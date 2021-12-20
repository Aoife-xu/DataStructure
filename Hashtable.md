# Hash tables

1 [【数据结构】5分钟带你搞定哈希表(建议收藏)!!! - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/376278867)

2 [来吧！一文彻底搞定哈希表！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/95156642)

3 http://java-awsome.blogspot.com/2017/07/how-hash-map-works-in-java.html



## Contents

* Hashtable
* Hash function
* key-value pair
* Hash collision
* Entry
* 

## 1.Hashtable

<img src="C:\Users\xuhan\AppData\Roaming\Typora\typora-user-images\image-20211217044314238.png" alt="image-20211217044314238" style="zoom:67%;" />

Definition: The major target of the hash tables is to minimize the searching time of any sort of data that needs to be fetched.

Function: The hash table searching the data by a key-value pair. Visit the value address in the memory directly by a Key. as Figure 9.1. Key----be calculated to a hash index(hash function)---> address of the value. quicker than mapping one by one. 



The essence of Hash table is an array.

<img src="C:\Users\xuhan\AppData\Roaming\Typora\typora-user-images\image-20211217032244588.png" alt="image-20211217032244588" style="zoom:80%;" />

There are two ways to implement hash table, array+linked list & array + tree. here is linked list.

key-value pair:

<img src="C:\Users\xuhan\AppData\Roaming\Typora\typora-user-images\image-20211217032732939.png" alt="image-20211217032732939" style="zoom:67%;" />

Figure 9.2



## 2. Hash function

Mod: 
$$
index = key  
MOD table size
$$
Truncation: 
$$
key = 123456 --> index = 3456
$$
Folding:
$$
key = 456789 --> index = 45+67+89 = 177
$$


## 3. Hash collision

### 3.1 An example of hash table

| key   | hash index | value |
| ----- | ---------- | ----- |
| Aoife | A(1)       | 001   |
| Alice | A(1)       | 004   |

select the first character as the hash index, two keys have same hash index which is the hash collision.

now the hash table is:

| indices    | 0    | 1(A)      | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---------- | ---- | --------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| keys-value |      | Aoife-001 |      |      |      |      |      |      |      |      |
|            |      | Alice-004 |      |      |      |      |      |      |      |      |

so a[1] is the what we are looking for.

### 3.2 Entry

key-value pair

key(head)---value--next(pointer) as Figure 9.3

```java
class HashEntry {
	String key;
	int value;

	// Reference to next entry
	HashEntry next;

	// Constructor
	public HashEntry(String key, int value) {
		this.key = key;
		this.value = value;
	}
}
```





### 3.3 Two types of the Hash table

Hash table stores the entries not keys. put entry into hash array at corresponding hash index by it's key.

101-->hash function--> 1-->a[1]

there are two ways to deal with hash collision: Linear probing and array with linked list

#### 3.3.1 linear probing

Figure 9.4

the entries need empty address to store, so search the empty in hash table. when existing the hash collision which means the index of the key already, and it's key is different to this. so search the next empty address to store the entry. in one word, search the empty in the array from the collision key to next, then next and so on.

the linear probing method is aiming to find a new position if it has been occupied. 

Three situations: EMPTY, never has been stored an entry.

​                               DELECTE, has ever stored an entry but has been deleted.

​                                EXIST, exist an entry currently.

#### 3.3.2 Linked list

Figure 9.5

a[1] not only stores an entry but also a pointer Next which point to next entry.

build the hash table

1 combine entries

```java
// Class to represent entire hash table
// Figure 9.7
class HashTable {
	private ArrayList < HashEntry > bucket;
	private int slots;
	private int size;

	//Constructor
	public HashTable() {
		bucket = new ArrayList < HashEntry >();
		slots = 10;
		size = 0;
		//initialize buckets <1>
		for (int i = 0; i < slots; i++)
		bucket.add(null);

	}
	public int size() {
		return size;
	}
	public boolean isEmpty() {
		return size() == 0;
	}
```

2 convert the key into hash

```java
private int getIndex(String key) {

	//hashCode is a built in function of Strings
	int hashCode = key.hashCode();
	int index = hashCode % slots;
	//Check if index is negative
	if(index<0){
		index=(index + slots) % slots; <2>
	}
	return index;
}
```



#### 3.3.3 Resizing

threshold: is the biggest size of the hash table that don't need to be changed, once it crossed, creating a new table which is double the size of the original. then copy the elements to our new table.

the purpose of resizing is to reduce the collisions.

0.6: the typically convention threshold which means 60% of table were occupied.

![image-20211217043013311](C:\Users\xuhan\AppData\Roaming\Typora\typora-user-images\image-20211217043013311.png)

main: create a new table then traverse the old one, in that, traverse each chain and insert the elements to new table from head. 

steps: 

   * create a new table, the size of old is 10 and the new is double to it which is 20.
   * judge if the head is null?
   * if the head node is not null,    head insert the elements to new table.

````java
	
    //Checks if array >60% of the array gets filled
		if ((1.0*size)/slots >= 0.6)
		{
			ArrayList<HashEntry> temp = bucket;
			bucket = new ArrayList();
			slots = 2 * slots;
			size = 0;
			for (int i = 0; i < slots; i++)
				bucket.add(null);

			for (HashEntry head_Node : temp)
			{
				while (head_Node != null)
				{
					insert(head_Node.key, head_Node.value);
					head_Node = head_Node.next;
				}
			}
		}
````



### 4 operations

#### 4.1 insertion

![image-20211217054323190](C:\Users\xuhan\AppData\Roaming\Typora\typora-user-images\image-20211217054323190.png)

<1> convert the key to hash index to find the chain node. 

<2> judge if the key is already exist, comparing the key. if exists, assign the value to it. if not, give a next pointer into head.

<3> put 1 to head, put new slot to as new entry.

<4> **head = head.next;:**   assign the address of the next node of head to head, so the head equals to the address of the next node. got the address of the node next to head.

 **new_slot.next = head;**    assign the address of the next node of head to new_slot's next node. so the next node got the address of head's next node. so it links the new slot and the next node.

so it links the new slot and 

````java
	//Inserts a key value pair into table
  // Figure 9.8
	public void insert(String key, int value)
{
		//Find head of the chain <1>
		int b_Index = getIndex(key);
		HashEntry head = bucket.get(b_Index);

		//Checks if the key is already exists <2>
		while(head != null)
		{
			if (head.key.equals(key))
			{
				head.value = value;
				return;
			}
			head = head.next;
		}

		// Inserts key in the chain < 3 >
		size++;
		head = bucket.get(b_Index);
		HashEntry new_slot = new HashEntry(key, value);
		new_slot.next = head; <4>
		bucket.set(b_Index, new_slot);

	
}
````



### 5 Hashtable implementation

#### 5.1 HashMap

`Map<Integer, Integer> map = new HashMap<>();`

function: mapping key-value pair in hash table. 

Map <keyType,valueType> Name = new HashMap<>();

HashMap is a hash table based implementation of java's Map interface.

examples:

creating a hashmap:

`Map<String, Integer> Map = new HashMap<>();`

adding key-value pairs:

`Map.put('one', '1');`

adding key-value pairs to it if the key does not exist:

`Map.putIfAbsent('two', 2);`

put vs putIfAbsent: if put the value to the same key, put will add the new value to the key while the putifabsent will not, the vaule will keeps the same.

methods:

- How to check if a HashMap is empty | [`isEmpty()`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#isEmpty--)
- How to find the size of a HashMap | [`size()`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#size--])
- How to check if a given key exists in a HashMap | [`containsKey()`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#containsKey-java.lang.Object-)
- How to check if a given value exists in a HashMap | [`containsValue()`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#containsValue-java.lang.Object-)
- How to get the value associated with a given key in the HashMap | [`get()`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#get-java.lang.Object-)
- How to modify the value associated with a given key in the HashMap | [`put()`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#put-K-V-)

```
getOrDefault(k, v): get the value mapped with specified key, if not returns default value
```

https://www.callicoder.com/java-hashmap/

https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#containsKey-java.lang.Object-

