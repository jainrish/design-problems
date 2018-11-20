### Design a Custom HashMap
```java
interface CustomMap<K, V> {
	public void put(K key, V value);
	public V get(K key);
	public void remove(K key);
	public boolean containsKey(K key);
}
public class CustomHashMap<K,V> implements CustomMap<K,V> {
	
	class Entry<K,V>{
		K key;
		V value;
		Entry<K,V> next;
		
		Entry(K key, V value){
			this.key = key;
			this.value = value;
		}
	}

	private Entry<K,V>[] entries;
	private static final Integer SIZE = 100;
	
	public CustomHashMap() {
		entries = new Entry[SIZE];
	}
	
	@Override
	public void put(K key, V value) {
		int pos = key.hashCode()%SIZE;
		Entry<K,V> entry = new Entry<K, V>(key, value);
		
		if(entries[pos]==null) {
			entries[pos] = entry;
		} else {
			addOrUpdate(entry, pos);
		}
		
	}

	@Override
	public V get(Object key) {
		int pos = key.hashCode()%SIZE;
		Entry<K,V> head = entries[pos];
		
		while(head!=null) {
			if(head.key.equals(key)) {
				return head.value;
			}
			head = head.next;
		}
		return null;
	}

	@Override
	public void remove(Object key) {
		int pos = key.hashCode()%SIZE;
		Entry<K,V> head = entries[pos], prev = head;
		if(head==null) return;
		
		if(head.key.equals(key)) {
			entries[pos] = head.next;
		}
		head = head.next;
		
		while(head!=null) {
			if(head.key.equals(key)) {
				prev.next = head.next;
				return;
			}
			prev = head;
			head = head.next;
		}
		
	}

	@Override
	public boolean containsKey(Object key) {
		int pos = key.hashCode()%SIZE;
		
		Entry<K,V> head = entries[pos];
		while(head!=null) {
			if(head.key.equals(key)) {
				return true;
			}
			head = head.next;
		}
		
		return false;
	}
	
	private void addOrUpdate(Entry<K,V> entry, int pos) {
		Entry<K,V> head = entries[pos], prev = entries[pos];
		while(head!=null) {
			if(head.key.equals(entry.key)) {
				head.value = entry.value;
				return;
			}
			prev = head;
			head = head.next;
		}
		prev.next = entry;
	}
	
	
}

```