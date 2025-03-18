- **hash table**은 **key, value 구조로 데이터를 저장하는 자료구조**입니다. **버킷**이라는 테이블에 key를 저장할 때 hash 함수의 결과물과 hash table 크기만큼 모듈러 연산을 한 해시 값을 이용합니다. **중복을 허용하지 않고** 탐색 시 **상수 시간으로 접근**하여 빠르다는 특징이 있습니다. 

## 해시 충돌
- 키를 해시 함수에서 연산하면 결과물로 해시 값이 나옵니다. 이때 다른 키를 해시 함수에다가 요청해도 같은 해시 값이 나오게 되는데 이를, **해시 충돌**이라고 합니다.

### 해시 충돌 해결법
- 해시 충돌을 해결하는 방법은 **Separate Chaining** 방식과 **Open addressing** 방식이 존재합니다. **Separate Chaining** 방식은 **링크드 리스트를 활용하여 추가 공간을 마련**하는 방식입니다. 
- **Open addressing** 방식은 **비어있는 인접한 공간에 저장하는 방식**입니다. 이중 해싱 방법이 있는 걸로 알고 있습니다.
## Hash Map과 Hash Table의 차이점
- `hashmap`은 **key와 value에 Null을 허용**한다. **thread-safe**하지 않아 싱글쓰레드 환경에 적합하다. 
- `hashTable`은 **key와 value에 null을 허용하지 않는다**. thread-safe 하기 때문에 싱글 쓰레드 환경에 적합하다. 하지만 데이터를 다룰 때마다 synchronized 키워드가 붙어있기 때문에 느리다는 단점이 있다.
- -> 보완하기 위해 나온 것이 `ConcurrencyHashMap`이다. key와 value에 Null을 허용하지 않고 thread-safe하며 Entry에 대해서만 lock을 걸기 때문에 hashTable 보다 속도가 빠르다.

### 왜 CourrentHashMap이 HashTable보다 성능이 빠를까요? 내부 동작에 대해 설명해주실 수 있나요?
- **임계영역 범위**가 다르기 때문입니다. `ConcurrentHashMap`은 우선 key, value에 null 값을 허용하지 않는다는 점에서 `HashTable`과 유사합니다. 다만 `HashTable`은 메소드에 synchronized가 붙어 있기 때문에 메소드를 사용하려고 시도하는 모든 시도마다 Lock이 걸리게 됩니다. 
- 하지만 `ConcurrentHashMap`은 저장된 엔트리를 기준으로 Lock을 걸기 때문에 같은 Entry를 변경하려면 Lock이 걸리지만 다른 엔트리를 사용할 때 Lock이 걸리지 않는다는 장점이 있습니다.

#### HashTable

```java
public class Hashtable<K,V> extends Dictionary<K,V> implements Map<K,V>, Cloneable, java.io.Serializable {

    ...

    public synchronized V put(K key, V value) {
        if (value == null) {
            throw new NullPointerException();
        }
        
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> entry = (Entry<K,V>)tab[index];
        for(; entry != null ; entry = entry.next) {
            if ((entry.hash == hash) && entry.key.equals(key)) {
                V old = entry.value;
                entry.value = value;
                return old;
            }
        }

        addEntry(hash, key, value, index);
        return null;
    }

    ...
}
```


#### ConcurrentHashMap
```java
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V> implements ConcurrentMap<K,V>, Serializable {

    ...

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) {
            throw new NullPointerException();
        }
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }

    ...
}
```