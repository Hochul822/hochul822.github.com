
- 리스트 뷰란?
  - 아이템
  - 레이아웃 연결
  - 어댑터

- 리사이클러 뷰란?
  - 레이아웃 매니저
  - 뷰홀더(View Holder)의 의미


### ListView

리스트 뷰는 한줄로 데이터를 보여주는 뷰에요.

세로로 된 데이터를 보여주고 싶을 때 많이 사용합니다.

리스트뷰의 구성요소는 3가지에요.

- 리스트뷰
  - 전체 뷰의 구성을 관리합니다.
- 아이템
  - 뷰에 있는 한칸 한칸을 담당합니다.
- 어댑터
  - 리스트뷰와 아이템을 연결하고, 어떤 모양으로 아이템을 표시할지 정하는 일을 합니다.

리스트뷰가 기존의 뷰와 다른 점은 중간에 어댑터가 있다는 건데요.

어댑터가 통로 역할을 해줍니다.

![alt text](img/04/listview.png)

직접 리스트뷰가 데이터들을 관리하려면 리스트뷰가 너무 커져서 이를 분리하기 위해서 나온 게 어댑터에요.  

리스트뷰를 만들며 어떻게 리스트뷰가 쓰이는지 알아보도록 할께요.

### 리스트뷰 만들기

###### 1. 레이아웃 설정
리스트뷰를 레이아웃에 더해줄께요.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_marginStart="14dp"
        android:layout_marginTop="32dp" />

</RelativeLayout>
```

###### 2. 아이템 만들기
Memo.java 클래스를 만들께요.

리스트뷰의 한칸 한칸에 쓰일 데이터에요.
```java
public class Memo {
    int no;
    String title;
    String content;

    public Memo(int no, String title, String content) {
        this.no = no;
        this.title = title;
        this.content = content;
    }

    public int getNo() {
        return no;
    }

    public String getTitle() {
        return title;
    }

    public String getContent() {
        return content;
    }
}
```

###### 3. 액티비티에 리스트뷰 추가하기
액티비티에 리스트뷰를 추가해줄께요.
밑의 코드를 따라하면은 에러가 날 거에요.

아직 어댑터가 없기 때문이에요.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ListView listView = findViewById(R.id.list_view);

        listView.setAdapter(adapter);
    }
}    
```

###### 4. 어댑터 만들기
MemoListAdapter란 클래스를 만들고
BaseAdapter를 상속해줄께요.

BaseAdapter는 기본적으로 많이 쓰는 것들을 담은 어댑터인데
커스텀할 수도 있어요.

```java
public class MemoListAdapter extends BaseAdapter {
    private Context context;
    private ArrayList<Memo> data;

    public MemoListAdapter(Context context, ArrayList<Memo> data) {
        this.context = context;
        this.data = data;
    }

    // 전체 뷰의 크기를 정하기 위함
    @Override
    public int getCount() {
        return data.size();
    }

    @Override
    public Object getItem(int position) {
        if (data.size() > 0)
            return data.get(position);

        return null;
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    // 각각의 데이터와 작은뷰들을 연결해준다.
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = LayoutInflater.from(context).inflate(R.layout.memo_title_item, parent, false);
        }

        TextView memo_title = convertView.findViewById(R.id.memo_title);
        memo_title.setText(data.get(position).getTitle());

        return convertView;
    }
}

```

###### 5. 어댑터 리스트뷰에 추가해주기
onCreate 내부에서 ArrayList를 만들고 필요한 데이터를 더해주도록 할께요.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


        ListView listView = findViewById(R.id.list_view);

        ArrayList<Memo> data = new ArrayList<Memo>();

        data.add(new Memo(1, "집에 가자", "왜 가야되지?"));
        data.add(new Memo(2, "스타벅스 들렀다가기", "가야되는 이유를 알겠네"));
        data.add(new Memo(3, "오늘은 뭘할까", "점심 먹기"));


        MemoListAdapter adapter = new MemoListAdapter(this, data);

        listView.setAdapter(adapter);
    }
}    
```

###### 6. 실행
무리없이 실행되면은 성공이에요!


###### 질문
- 나중에 데이터를 추가하면 어떻게 리스트뷰를 업데이트할까요?
- 데이터를 어디서 추가해주는 게 좋을까요? onCreate에서 추가하는 게 과연 좋을까요?
