# Comparator , Comparable


### Comparable
> - 클래스의 기본 정렬 기준을 설정해주는 인터페이스
> - Comparable 인터페이스를 implements 한 뒤, 내부에 있는 compareTo 메서드를 정의

```java
import java.lang.Comparable; //패키지 import

class Student implements Comparable<Student> { //제너릭스 주의!
	String name; //이름
	int id; //학번
	double score; //학점
	public Student(String name, int id, double score){
		this.name = name;
		this.id = id;
		this.score = score;
	}
	public String toString(){ //출력용 toString오버라이드
		return "이름: "+name+", 학번: "+id+", 학점: "+score;
	}
    @Override
    public int compareTo(Student anotherStudent) {
        // TODO Auto-generated method stub
        return Integer.compare(id, anotherStudent.id);
    }
}

```
- Integer.compare 는 단순히 첫번째 매개변수와 두번째 매개변수가 오름차순으로 유지될 수 있도록 값을 비교해주는메서드,
- return 값이 0이나 음수이면 자리바꿈을 하지 않고, 양수이면 자리바꿈을 실행하는 것,
- 만약 오름차순이 아닌 내림차순으로 정렬하고 싶다면, 매개변수의 순서를 바꿔주면 된다
- Comparable 구현되지 않은 상태에서 Arrays.sort() 또는 Collection.sort() 사용시 에러발생한다.



### Comparator
> - 기본 정렬 기준과는 다르게 정렬하고 싶을 때 이용하는 클래스
> - Comparator 클래스를 생성하여 내부에 compare 메서드를 원하는 정렬 기준대로 구현
> - 주로 익명클래스로 사용되며, 이를 Arrays.sort() 또는 Collection.sort()내부에 구현하면된다.
> - 람다식으로 구현하는게 깔끔함.

