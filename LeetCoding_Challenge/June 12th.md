### Problem 
- [Insert Delete GetRandom O(1)](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge/540/week-2-june-8th-june-14th/3358/)


### 해결책
- HashMap: HashTable이기에 Insert Delete Average O(1)
- ArrayList: Random access를 하기 위해서 생성.
- HashMap의 value는 ArrayList에 Idx 값을 가지고 있고, remove할 때 이를 찾아서 삭제하는 방식
  * uniform하게 랜덤값을 뽑아내기 위해 arraylist에서 remove할 때는 맨 마지막과 swap하여 진행.
- Java 랜덤값 발생 방법: Math.Random() vs Random Class
  * Math.Random()은 0.0 <= x < 1.0 사이의 값을 랜덤으로 뱉어내고, 객체를 생성하지 않고 바로 사용할 수 있는 장점이 있다. 하지만 자신이 원하는 값의 범위로 
  변환하기에 해당 부분은 단점이다.
  * java.util.Random은 난수 발생시 seed값 지정이 가능하고, 다른 자료형으로 변환하는 것이 용이하다.
  * 사실 Math.Random()가 synchronized를 지원하지만 많은 thread에서 사용할 때 thread content(unlock될 때까지 대기하는 현상)이 발생할 수 있다. 따라서
  JDK 1.7에서부터 ThreadLocalRandom을 지원하기에 이를 사용하는 것이 성능상 이점이 있다.


```java
import java.util.*;

class RandomizedSet {
    private Map<Integer,Integer> map; //value는 arrayList의 idx
    private List<Integer> arrayList; //value는 원래 값

    /** Initialize your data structure here. */
    public RandomizedSet() {
        map = new HashMap<>();
        arrayList = new ArrayList<>();
    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if(map.containsKey(val)){ //AutoBoxing
            return false;
        }

        map.put(val, arrayList.size());
        arrayList.add(val);
        return true;
    }

    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        if(map.containsKey(val)){
            int findRemoveIdx = map.get(val);
            int listSize = arrayList.size();
            //교체
            arrayList.set(findRemoveIdx, arrayList.get(listSize - 1));
            int fixedIdx = arrayList.get(findRemoveIdx);
            map.replace(fixedIdx, findRemoveIdx);

            //삭제
            arrayList.remove(listSize - 1);
            map.remove(val);

            return true;
        }
        return false;
    }

    /** Get a random element from the set. */
    public int getRandom() {
        Random random = new Random();
        return arrayList.get(random.nextInt(arrayList.size()));
    }
}
```



### 참고자료
- Leetcode
- [HashSet contains() Method in Java](https://www.geeksforgeeks.org/hashset-contains-method-in-java/)
- [[Java] 자바의 자료구조 - Set(HashSet), Queue(LinkedList)](https://onsil-thegreenhouse.github.io/programming/java/2018/02/21/java_tutorial_1-23/)
- [Java Summary: Math.random() and java.util.Random](http://www.fredosaurus.com/notes-java/summaries/summary-random.html)
- [[JAVA] Random 클래스에 대해서](https://hyeonstorage.tistory.com/160)
