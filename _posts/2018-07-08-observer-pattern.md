---
layout: post
title: '[Design Pattern] 옵저버 패턴이란'
subtitle: '옵저버 패턴을 이해한다.'
date: 2018-07-08
author: heejeong Kwon
cover: '/images/design-pattern-observer/observer-main.png'
tags: 디자인패턴 design-pattern observer
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 옵저버 패턴의 개념을 이해한다.
> - 예시를 통해 옵저버 패턴을 이해한다.

## 옵저버 패턴이란
* 한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 **일대다 객체 의존 관계를 구성** 하는 패턴
  * 데이터의 변경이 발생했을 경우 **상대 클래스나 객체에 의존하지 않으면서 데이터 변경을 통보하고자 할 때** 유용하다.
    * Ex) 새로운 파일이 추가되거나 기존 파일이 삭제되었을 때 탐색기는 다른 탐색기에게 즉시 변경을 통보해야 한다.
    * Ex) 차량 연료량 클래스는 연료량이 부족한 경우 연료량에 관심을 가지는 구체적인 클래스(연료량 부족 경고 클래스, 주행 가능 거리 출력 클래스)에 직접 의존하지 않는 방식으로 연료량 변화를 통보해야 한다.
  * '행위(Behavioral) 패턴'의 하나 (*아래 참고*)
* ![](/images/design-pattern-observer/observer-pattern.png)
* 옵저버 패턴은 통보 대상 객체의 관리를 Subject 클래스와 Obaserver 인터페이스로 일반화한다.
  * 이를 통해 데이터 변경을 통보하는 클래스(ConcreteSubject)는 통보 대상 클래스나 객체(ConcreteObserver)에 대한 의존성을 없앨 수 있다.
  * 결과적으로 통보 대상 클래스나 대상 객체의 변경에도 **통보하는 클래스(ConcreteSubject)를 수정 없이 그대로 사용할 수 있다.**
* 역할이 수행하는 작업
  * Observer
    * 데이터의 변경을 통보 받는 인터페이스
    * 즉, Subject에서는 Observer 인터페이스의 update 메서드를 호출함으로써 ConcreteSubject의 데이터 변경을 ConcreteObserver에게 통보한다.
  * Subject
    * ConcreteObserver 객체를 관리하는 요소
    * Observer 인터페이스를 참조해서 ConcreteObserver를 관리하므로 ConcreteObserver의 변화에 독립적일 수 있다.
  * ConcreteSubject
    * 변경 관리 대상이 되는 데이터가 있는 클래스(통보하는 클래스)
    * 데이터 변경을 위한 메서드인 setState가 있다.
    * setState 메서드에서는 자신의 데이터인 subjectState를 변경하고 Subject의 notifyObservers 메서드를 호출해서 ConcreteObserver 객체에 변경을 통보한다.
  * ConcreteObserver
    * ConcreteSubject의 변경을 통보받는 클래스
    * Observer 인터페이스의 update 메서드를 구현함으로써 변경을 통보받는다.
    * 변경된 데이터는 ConcreteSubject의 getState 메서드를 호출함으로써 변경을 조회한다.

<mark>참고</mark>
* 행위(Behavioral) 패턴
  * 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
  * 한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둔다.


## 예시
### 여러 가지 방식으로 성적 출력하기
* ![](/images/design-pattern-observer/observer-example.png)
* 입력된 성적 값을 출력하는 프로그램

~~~Java
/* 입력된 점수를 저장하는 클래스 (1. 출력형태: 목록) */
public class ScoreRecord {
  private List<Integer> scores = new ArrayList<Integer>(); // 점수를 저장함
  private DataSheetView dataSheetView; // 목록 형태로 점수를 출력하는 클래스

  public void setDataSheetView(DataSheetView dataSheetView) { this.dataSheetView = dataSheetView; }
  // 새로운 점수를 추가하면 출력하는 것에 변화를 통보(update())하여 출력하는 부분 갱신
	public void addScore(int score) {
	   scores.add(score); // scores 목록에 주어진 점수를 추가함
	   dataSheetView.update(); // scores가 변경됨을 통보함
	}
	// 출력하는 부분에서 변화된 내용을 얻어감
	public List<Integer> getScoreRecord() { return scores; }
}
~~~

~~~Java
/* 1. 출력형태: 목록 형태로 출력하는 클래스 */
public class DataSheetView {
	private ScoreRecord scoreRecord;
	private int viewCount;

	public DataSheetView(ScoreRecord scoreRecord, int viewCount) {
		this.scoreRecord = scoreRecord;
		this.viewCount = viewCount;
	}

	// 점수의 변경을 통보받음
	public void update() {
		List<Integer> record = scoreRecord.getScoreRecord(); // 점수를 조회함
		displayScores(record, viewCount); // 조회된 점수를 viewCount 만큼만 출력함
	}

	private void displayScores(List<Integer> record, int viewCount) {
		System.out.println("List of " + viewCount + " entries: ");
		for (int i = 0; i < viewCount && i < record.size(); i++) {
			System.out.println(record.get(i) + " ");
		}
		System.out.println();
	}
}
~~~

~~~Java
public class Client {
	public static void main(String[] args) {
		ScoreRecord scoreRecord = new ScoreRecord();

		// 3개까지의 점수만 출력함
		DataSheetView dataSheetView = new DataSheetView(scoreRecord, 3);
		scoreRecord.setDataSheetView(dataSheetView);

		for (int index = 1; index <= 5; index++) {
			int score = index * 10;
			System.out.println("Adding " + score);

			// 10 20 30 40 50을 추가함, 추가할 때마다 최대 3개의 점수만 출력함
			scoreRecord.addScore(score);
		}
	}
}
~~~
* ScoreRecord 클래스의 addScore 메서드가 호출되면 ScoreRecord 클래스는 자신의 필드인 scores 객체에 점수를 추가한다.
  * 그리고 DataSheetView 클래스의 update 메서드를 호출함으로써 성적을 출력하도록 요청한다.
* DataSheetView 클래스는 ScoreRecord 클래스의 getScoreRecord 메서드를 호출해 출력할 점수를 구한다.
  * 이때 DataSheetView 클래스의 update 메서드에서는 구한 점수 중에서 명시된 개수(viewCount) 만큼의 점수만 출력한다.

### 문제점
1. 성적을 다른 형태로 출력하는 경우
  * 성적을 목록으로 출력하지 않고 성적의 최소/최대 값만 출력하려면?
~~~Java
/* 입력된 점수를 저장하는 클래스 (2. 출력형태: 최대,최소값) */
public class ScoreRecord {
    private List<Integer> scores = new ArrayList<Integer>(); // 점수를 저장함
    private MinMaxView minMaxView; // 최소/최대 값만을 출력하는 형태의 클래스
    // MinMaxView를 설정함
    public void setMinMaxView(MinMaxView minMaxView) { this.minMaxView = minMaxView; }
    // 새로운 점수를 추가하면 출력하는 것에 변화를 통보(update())하여 출력하는 부분 갱신
    public void addScore(int score) {
          scores.add(score); // scores 목록에 주어진 점수를 추가함
          minMaxView.update(); // MinMaxView에게 scores가 변경됨을 통보함
    }
    // 출력하는 부분에서 변화된 내용을 얻어감
    public List<Integer> getScoreRecord() { return scores; }
}
~~~
~~~Java
/* 2. 출력형태: 최소/최대 값만을 출력하는 형태의 클래스 */
public class MinMaxView {
  private ScoreRecord scoreRecord;
  // getScoreRecord()를 호출하기 위해 ScoreRecord 객체를 인자로 받음
  public MinMaxView(ScoreRecord scoreRecord) {
      this.scoreRecord = scoreRecord;
  }
  // 점수의 변경을 통보받음
  public void update() {
      List<Integer> record = scoreRecord.getScoreRecord(); // 점수를 조회함
      displayScores(record); // 최소값과 최대값을 출력함
  }
  // 최소값과 최대값을 출력함
  private void displayScores(List<Integer> record) {
      int min = Collections.min(record, null);
      int max = Collections.max(record, null);
      System.out.println("Min: " + min + ", Max: " + max);
  }
}
~~~
  * 점수 변경에 대한 통보 대상 클래스가 다른 대상 클래스(DataSheetView->MinMaxView)로 바뀌면 기존 코드(ScoreRecord 클래스)의 내용을 수정해야 하므로 **OCP에 위배된다.**
2. 동시 혹은 순차적으로 성적을 출력하는 경우
  * 성적이 입력되었을 때 최대 3개 목록, 최대 5개 목록, 최소/최대 값을 동시에 출력하려면?
  * 처음에는 목록으로 출력하고 나중에는 최소/최대 값을 출력하려면?
~~~java
/* 입력된 점수를 저장하는 클래스 (3. 출력형태: 2개 출력 형태를 가질 때) */
public class ScoreRecord {
  private List<Integer> scores = new ArrayList<Integer>(); // 점수를 저장함
  private DataSheetView dataSheetView; // 목록 형태로 점수를 출력하는 클래스
  private MinMaxView minMaxView; // 최소/최대 값만을 출력하는 형태의 클래스
  // DataSheetView를 설정함
  public void setDataSheetView(DataSheetView dataSheetView) { this.dataSheetView = dataSheetView; }
  // MinMaxView를 설정함
  public void setMinMaxView(MinMaxView minMaxView) { this.minMaxView = minMaxView; }
  // 새로운 점수를 추가하면 출력하는 것에 변화를 통보(update())하여 출력하는 부분 갱신
  public void addScore(int score) {
    scores.add(score); // scores 목록에 주어진 점수를 추가함
    dataSheetView.update(); // scores가 변경됨을 통보함
    minMaxView.update(); // scores가 변경됨을 통보함
  }
  // 출력하는 부분에서 변화된 내용을 얻어감
  public List<Integer> getScoreRecord() {
    return scores;
  }
}
~~~
~~~Java
public class Client {
  public static void main(String[] args) {
      ScoreRecord scoreRecord = new ScoreRecord();
      // 3개까지의 점수만 출력함
      DataSheetView dataSheetView = new DataSheetView(scoreRecord, 3);
      // 최대값, 최소값만 출력함
      MinMaxView minMaxView = new MinMaxView(scoreRecord);
      // 각 통보 대상 클래스를 저장
      scoreRecord.setDataSheetView(dataSheetView);
      scoreRecord.setMinMaxView(minMaxView);
      // 10 20 30 40 50을 추가
      for (int index = 1; index <= 5; index++) {
            int score = index * 10;
            System.out.println("Adding " + score);
            // 추가할 때마다 최대 3개의 점수 목록과 최대/최소값이 출력됨
            scoreRecord.addScore(score);
      }
  }
}
~~~
  * 이 경우에도 점수 변경에 대한 통보 대상 클래스가 다른 대상 클래스(DataSheetView->MinMaxView)로 바뀌면 기존 코드(ScoreRecord 클래스)의 내용을 수정해야 하므로 **OCP에 위배된다.**
  * 즉, 성적 변경을 새로운 클래스에 통보할 때마다 ScoreRecord 클래스의 코드를 수정해야 하므로 재사용하기 어렵다.

* ![](/images/design-pattern-observer/observer-problem.png)

### 해결책
문제를 해결하기 위해서는 <span style="background-color: #e1e1e1">공통 기능을 상위 클래스 및 인터페이스로 일반화</span> 하고 이를 활용하여 통보하는 클래스(ScoreRecord 클래스)를 구현해야 한다.
* 즉, ScoreRecord 클래스에서 변화되는 부분을 식별하고 이를 일반화시켜야 한다.
* 이를 통해 성적 통보 대상이 변경되더라도 ScoreRecord 클래스를 그대로 재사용할 수 있다.
* ScoreRecord 클래스에서 하는 작업
  * 통보 대상인 객체를 참조하는 것을 관리(추가/제거) -> **Subject** 클래스로 일반화
  * addScore 메서드: 각 통보 대상인 객체의 update 메서드를 호출  -> **Observer** 인터페이스로 일반화
* ![](/images/design-pattern-observer/observer-solution.png)

1. ScoreRecord 클래스의 addScore(상태 변경) 메서드 호출
  * 1-1. 자신의 성적 값 저장
  * 1-2. 상태가 변경될 때마다 Subject 클래스의 notifyObservers 메서드 호출
2. Subject 클래스의 notifyObservers 메서드 호출
  * Observer 인터페이스를 통해 성적 변경을 통보
  * 2-1. DataSheetView 클래스의 update 메서드 호출
  * 2-2. MinMaxView 클래스에 update 메서드 호출

* Observer 인터페이스
~~~Java
/* 추상화된 통보 대상 */
public interface Observer {
    // 데이터 변경을 통보했을 때 처리하는 메서드
    public abstract void update();
}
~~~
* Subject 클래스
~~~java
/* 추상화된 변경 관심 대상 데이터 */
// 즉, 데이터에 공통적으로 들어가야하는 메서드들 -> 일반화
public abstract class Subject {
    // 추상화된 통보 대상 목록 (즉, 출력 형태에 대한 Observer)
    private List<Observer> observers = new ArrayList<Observer>();

    // 통보 대상(Observer) 추가
    public void attach(Observer observer) { observers.add(observer);}
    // 통보 대상(Observer) 제거
    public void detach(Observer observer) { observers.remove(observer);}
    // 각 통보 대상(Observer)에 변경을 통보. (List<Observer>객체들의 update를 호출)
    public void notifyObservers() {
        for (Observer o : observers) {
            o.update();
        }
    }
}
~~~
* ScoreRecord 클래스
~~~Java
/* 구체적인 변경 감시 대상 데이터 */
// 출력형태 2개를 가질 때
public class ScoreRecord extends Subject{
	private List<Integer> scores = new ArrayList<Integer>(); // 점수를 저장함
	// 새로운 점수를 추가 (상태 변경)
	public void addScore(int score) {
		scores.add(score); // scores 목록에 주어진 점수를 추가함
		notifyObservers(); // scores가 변경됨을 각 통보 대상(Observer)에게 통보함
	}
	public List<Integer> getScoreRecord() { return scores; }
}
~~~
* DataSheetView 클래스
~~~Java
/* 통보 대상 클래스 (update 메서드 구현) */
// 1. 출력형태: 목록 형태로 출력하는 클래스
public class DataSheetView implements Observer{
	// 위와 동일
}
~~~
* MinMaxView 클래스
~~~Java
/* 통보 대상 클래스 (update 메서드 구현) */
// 2. 출력형태: 최대값 최소값만 출력하는 클래스
public class MinMaxView implements Observer{
	// 위와 동일
}
~~~
* 클라이언트에서의 사용
~~~Java
public class Client {
    public static void main(String[] args) {
        ScoreRecord scoreRecord = new ScoreRecord();

        // 3개까지의 점수만 출력함
        DataSheetView dataSheetView = new DataSheetView(scoreRecord, 3);
        // 최대값, 최소값만 출력함
        MinMaxView minMaxView = new MinMaxView(scoreRecord);

        // 각 통보 대상 클래스를 Observer로 추가
        scoreRecord.attach(dataSheetView);
        scoreRecord.attach(minMaxView);

        // 10 20 30 40 50을 추가
        for (int index = 1; index <= 5; index++) {
            int score = index * 10;
            System.out.println("Adding " + score);
            // 추가할 때마다 최대 3개의 점수 목록과 최대/최소값이 출력됨
            scoreRecord.addScore(score);
        }
    }
}
~~~
* Observer
  * 추상화된 통보 대상
* DataSheetView, MinMaxView
  * Observer를 implements함으로써 구체적인 통보 대상이 됨
* Subject
  * 성적 변경에 관심이 있는 대상 객체들을 관리
* ScoreRecord
  * Subject를 extends함으로써 구체적인 통보 대상을 직접 참조하지 않아도 됨
* 이렇게 Observer 패턴을 이용하면 ScoreRecord 클래스의 코드를 변경하지 않고도 새로운 관심 클래스 및 객체를 추가/제거하는 것이 가능해진다.


* ![](/images/design-pattern-observer/observer-solution2.png)



# 관련된 Post
* 스트래티지(Strategy) 패턴에 대해 알고 싶으시면 [스트래티지(Strategy) 패턴](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)을 참고하시기 바랍니다.
* 싱글턴(Singleton) 패턴에 대해 알고 싶으시면 [싱글턴(Singleton) 패턴](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)을 참고하시기 바랍니다.
* 커맨드(Command) 패턴에 대해 알고 싶으시면 [커맨드(Command) 패턴](https://gmlwjd9405.github.io/2018/07/07/command-pattern.html)을 참고하시기 바랍니다.
<!-- * 옵저버(Observer) 패턴에 대해 알고 싶으시면 [옵저버(Observer) 패턴](https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html)을 참고하시기 바랍니다. -->
* 데코레이터(Decorator) 패턴에 대해 알고 싶으시면 [데코레이터(Decorator) 패턴](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)을 참고하시기 바랍니다.
* 템플릿 메서드(Template Method) 패턴에 대해 알고 싶으시면 [템플릿 메서드(Template Method) 패턴](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)을 참고하시기 바랍니다.
* 팩토리 메서드(Factory Method) 패턴에 대해 알고 싶으시면 [팩토리 메서드(Factory Method) 패턴](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)을 참고하시기 바랍니다.
* 추상 팩토리(Abstract Factory) 패턴에 대해 알고 싶으시면 [추상 팩토리(Abstract Factory) 패턴](https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html)을 참고하시기 바랍니다.
* 컴퍼지트(Composite) 패턴에 대해 알고 싶으시면 [컴퍼지트(Composite) 패턴에](https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html)을 참고하시기 바랍니다.


# References
> - [JAVA 객체지향 디자인 패턴, 한빛미디어](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968480911&orderClick=JAj)
