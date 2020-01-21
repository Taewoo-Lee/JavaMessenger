# 자바 메신저(Java Messenger)
&nbsp;메신저 프로그램은 사용자의 회원 가입 및 로그인 DB를 이용하여 클라이언트 간의 메시지를 중계하는 서버 프로그램과 클라이언트 프로그램으로 구성된다. 이 메신저 프로그램의 핵심은 서버 프로그램의 설계에 있다. 클라이언트와 서버 간의 통신은 어차피 인터넷을 이용하기 때문에 프로그램 자체는 직접적으로 연관되지 않는다. 단지 네트워크로 주고받는 메시지의 규격만 맞으면 어떤 클라이언트와도 서버와 통신이 가능한 구조이다. 따라서 서버를 잘 설계하고 메시지 규격(프로토콜)을 공개한다면, 다양한 클라리언트를 지원하는 것도 그리 어렵지 않다.<br>

&nbsp;물론 대규모 사용자를 고려한 서버 프로그램이라면 안정성과 성능 등 요소를 충족해야 하므로 책에서 배운 소켓 프로그래밍 수준 이상의 네트워크 프로그래밍 기술이 요구된다. 여기서는 지금까지 배운 기본적인 네트워크, 입출력, 스레드, GUI, 자료구조 등 프로그래밍 기술을 총동원해서 다중 클라이언트를 지원하는 멀티 채팅 서버와 클라이언트를 구현할 것이다.<br>

&nbsp;이 자바 메신저는 JUST JAVA(황희정 지음, 한빛아카데미) 책을 바탕으로 하고 있다.

<br>

# 환경 구축
&nbsp;자바 프로그램을 개발하려면 기본적으로 자바 가상머신과 컴파일러, 라이브러리, 런처, 디버거 등 여러 요소로 구성된 JDK(Java Devlopment Kit)가 필요하다. <br>JDK만으로 소스 컴파일과 실행이 가능하지만 소스 편집과 관리, 컴파일, 실행, 라이브러리 고나리 등을 간편하게 하려면 통합 개발 환경이 필요하다. <br>이클립스는 대표적인 자바 개발 환경이다.

## JDK 설치
&nbsp;단순하게 자바 프로그램을 실행하는 데 JDK가 아닌 JRE만 있어도 충분하다. 또한 컴퓨터 사양에 별다른 제한은 없지만, 원활하게 사용하려면 최소 1GB 이상의 램을 확보하는 것이 좋다.<br>
1. developer.oracle.com 접속 후 `Downloads > Java SE`

	<img src="https://user-images.githubusercontent.com/41332126/72773972-4588dd80-3c4c-11ea-9b37-857fc0347ca3.png" width=630>
2. `Java Platform, Standard Edition 13` 또는 `Java SE 13` 를 선택한다. 둘 다 같은 화면으로 이동한다.<br>JDK 버전은 다운로드하는 시점에 따라 다를 수 있다.

	<img src="https://user-images.githubusercontent.com/41332126/72774273-4e2de380-3c4d-11ea-9636-158d874d2189.png" width=630>
3. `Java Platform (JDK) 13`

	<img src="https://user-images.githubusercontent.com/41332126/72774666-d19c0480-3c4e-11ea-818e-2010cac49375.png" width=630>
4. `Accept License Agreement 체크 > jdk-13.0.2_windows-x64_bin.exe` 자신의 운영체제에 맞는 파일을 다운로드한다.

	<img src="https://user-images.githubusercontent.com/41332126/72775119-7b2fc580-3c50-11ea-80c7-84194f90e08d.png" width=630>
5. 다운로드한 파일을 실행한 후 `Next`

	<img src="https://user-images.githubusercontent.com/41332126/72775421-9fd86d00-3c51-11ea-84a0-306f0950e53c.png">
6. 설치할 폴더를 확인하고 `Next`

	<img src="https://user-images.githubusercontent.com/41332126/72775471-d615ec80-3c51-11ea-98c7-d23b40e5c193.png">
7. `Close`
8. 명령 프롬포트(cmd) 창을 연다. 파일을 설치했던 경로로 가서 `java`를 입력해서 java 명령어가 출력되거나 <br>`java -version`을 입력해서 현재 버전이 출력된다면 JDK가 성공적으로 설치된 것이다.

	<img src="https://user-images.githubusercontent.com/41332126/72775786-132eae80-3c53-11ea-9fdd-3daafbf11123.png">
	<img src="https://user-images.githubusercontent.com/41332126/72775978-cbf4ed80-3c53-11ea-801c-fff27e31ed25.png">
	
	만약 위와 같이 출력되지 않고 아래와 같이 나온다면 환경변수를 통해 직접 추가하는 방법을 이용한다.
	```	
	'java'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램 또는 배치파일이 아닙니다.
	```
<br>

## Eclipse
&nbsp;자바는 Sun Microsystems에서 개발한 프로그래밍 언어로 태양(sun)과 관련 있는 식(蝕), 개기일식을 의미하는 이클립스를 이름으로 지었다.<br>
이클립스는 Windows, Mac OS X 및 Linux와 같은 주요 운영 체제 어디에서든 제한 없이 사용이 가능하다 보니 다양한 플랫폼의 소프트웨어 개발에 쓰이고 있다.

<img src="https://dora-guide.com/wp-content/uploads/2019/10/eclipse-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EC%9D%B4%ED%81%B4%EB%A6%BD%EC%8A%A4-%EC%84%A4%EC%B9%98.png.webp" height=150>
<br><br>

## 이클립스 설치
> 설치가이드 www.eclipse.org/downloads/packages/installer
1. www.eclipse.org/downloads 접속 후 `Download 64 bit`

    <img src="https://user-images.githubusercontent.com/41332126/72130055-b8c46100-33bb-11ea-8f6c-f149ae6c62f5.png" height=200>
2. `Download` eclipse-inst-win64.exe

    <img src="https://user-images.githubusercontent.com/41332126/72130197-1789da80-33bc-11ea-973a-4719cbb934dd.png" height=150>
3. 개발 목적에 따라 다양한 IDE들을 설치할 수 있다.<br>
자바 개발을 하기 위해 **`Eclipse IDE for java Developers`** 를 클릭하여 다운로드.<br>
아래 설치 파일들은 필요한 경우 나중에 플러그인을 통해 다른 기능도 설치할 수 있다.

    <img src="https://user-images.githubusercontent.com/41332126/72131742-7b160700-33c0-11ea-8d27-f42b8d3ad464.png" height=450>
4. `INSTALL` 기본 경로로 설치를 계속한다. 설치 경로를 따로 지정할 수도 있다.<br>
아래 두 옵션은 사용자 편의에 따라 임의대로 지정한다.
- Create start menu entry : 시작 메뉴에 추가하기
- Create desktop shortcut : 바탕화면에 바로가기 만들기

    <img src="https://user-images.githubusercontent.com/41332126/72132173-a77e5300-33c1-11ea-8a64-8218f890edf6.png" height=450>
5. 움프(Oomph) 라이센스 동의. `체크 후 Accept`<br>움프라 하는 새로운 프로젝트는 IDE를 좀 더 쉽게 설치할 수 있게 하고 프로젝트 공간을 프로비저닝한다.

    <img src="https://user-images.githubusercontent.com/41332126/72133051-e2818600-33c3-11ea-9618-0d759de8e000.png" height=400>
6. 인증서 동의. `Select All 후 Accept selected`

    <img src="https://user-images.githubusercontent.com/41332126/72134163-a00d7880-33c6-11ea-89ba-8249b3cb5bba.png" height=450>
7. `LAUNCH` 

    <img src="https://user-images.githubusercontent.com/41332126/72134541-b9fb8b00-33c7-11ea-9cf5-03bac6e29db5.png" height=450>
<br>


## 이클립스 환경 설정
1. `Launch` 기본 경로로 연다. 작업 경로를 따로 지정할 수도 있다.
- Use this as the default and do not ask again : 경로를 고정하고 이 팝업창을 다시 띄우지 않기

    <img src="https://user-images.githubusercontent.com/41332126/72135201-40fd3300-33c9-11ea-9105-91e855b86358.png" height=230>
2. Welcome 창을 닫거나 Workbench를 클릭하여 작업을 시작한다.
- Always show Welcome at start up : 프로그램 시작 시 항상 Welcome 창 띄우기

    <img src="https://dora-guide.com/wp-content/uploads/2019/10/10.png" height=450>

### 프로젝트 다운로드
3. 현재 깃허브 페이지에 업로드돼있는 파일을 Git Bash에서 `clone 주소` 하거나 `Download ZIP`을 통해 원하는 드라이브 경로에 저장한 후 압축을 풀어준다. github.com/taeyoung98/JavaMessenger

    <img src="https://user-images.githubusercontent.com/41332126/72137801-cf27e800-33ce-11ea-9ee8-9d5a680a6a03.png" height=350>
4. 다시 이클립스로 돌아와서 File > Open Projects form File System...
5. `Directory` 를 위에서 파일 저장해뒀던 경로로 지정하고 `Finish` 한다.

    <img src="https://user-images.githubusercontent.com/41332126/72138874-12835600-33d1-11ea-8d82-e8e908460c2d.png" height=450>

### 한글 설정
&nbsp;그러나 한글이 깨지는 경우가 발생할 수 있다. 이런 경우는 import한 프로젝트는 **UTF-8**으로 설정돼있지만 이클립스 인코딩(encoding)이 "EUC-KR" 또는 "MS949"로 설정돼있기 때문이다.
> 이클립스 인코딩 설정
6. Winodw > Preferences > General > Content Types > Java Class File 선택 > `Default encoding에` **` UTF-8 `** `입력`

    <img src="https://user-images.githubusercontent.com/41332126/72330901-a703f600-36fa-11ea-8c7a-f2c1621460c1.png" height=350>
7. Window > Preferences > General > Editors > Text Editors > Spelling > `Encoding에서` **` UTF-8 `** `선택`

    <img src="https://user-images.githubusercontent.com/41332126/72330964-c00ca700-36fa-11ea-9e6f-e280f12e18e3.png" height=550>
8. Window > Preferences > General > Workspace > `Text file encoding에서` **` UTF-8 `** `선택`

    <img src="https://user-images.githubusercontent.com/41332126/72331040-d9155800-36fa-11ea-8c30-2a5a7936fa71.png" height=480>
> 프로젝트 인코딩 설정
9. Project > Properties > Resource > `Text file encoding에서` **` UTF-8 `** `선택`

    <img src="https://user-images.githubusercontent.com/41332126/72331145-0b26ba00-36fb-11ea-9554-91552ec46b77.png" height=350>
10. 클라이언트에 해당하는 MultiChatController.java와 서버에 해당하는 MultiChatServer.java 파일을 열고 각각 `Run(Ctrl+F11)`시켜준다.

    <img src="https://user-images.githubusercontent.com/41332126/72335053-ade23700-3701-11ea-9bca-23ef5e612079.png" height=350>
    <img src="https://user-images.githubusercontent.com/41332126/72336389-0d414680-3704-11ea-978b-7f0e83e4ac91.png" height=400>
<br>

## Window Builder
&nbsp;우선 필자는 윈도우빌더로 만든 파일 자체를 이용하지 않고 일부 코드만을 가져와 참조하는 용도로 사용하였다.<br>
윈도우빌더는 자바 GUI 응용 프로그램을 만들 때 보다 더 가시적으로 해주어 사용자의 GUI 작업을 보다 더 편리하게 도와주는 도구이다. <br>SWT Designer와 Swing Designer로 구성되어 있으며, 컨트롤을 끌어놓아 쉽게 추가하고 컨트롤에 이벤트 핸들러를 추가하고 속성 편집기를 사용하여 <br>컨트롤의 다양한 속성을 변경하는 등 다양한 기능을 제공한다. 따라서 이전에 GUI 코드를 모두 직접 타이핑해야는 수고로움과 작성 동시에 <br>확인할 수 없다는 번거러움을 해소하고 코드 작성에 많은 시간을 소비하지 않고도 Java GUI 응용 프로그램을 매우 쉽게 작성할 수 있게 해준다.
### 설치 방법
1. Help > Eclipse Marketplace...

    <img src="https://t1.daumcdn.net/cfile/tistory/9913E53E5BB891FB1A" height=300>
2. `Windowbuilder 검색 후 Install`

    <img src="https://user-images.githubusercontent.com/41332126/72404695-99ea1400-3799-11ea-9111-95df0f66d0ec.png" height=330>
3. 설치할 항목을 선택 후 `Confirm`

    <img src="https://t1.daumcdn.net/cfile/tistory/992DE83E5BB891FC27" height=600>
4. 라이센스 동의. `선택 후 Finish`

    <img src="https://t1.daumcdn.net/cfile/tistory/9953573E5BB891FD26" height=400>
5. 이클립스 재시작. `Yes`<br>
설치가 끝나면 이클립스 재시작 여부를 묻는 팝업 등장

    <img src="https://t1.daumcdn.net/cfile/tistory/2664243F564EFB0B0C" height=130>

### 사용 방법
6. File > New > Other... (Ctrl+N)
7. WidowBuilder > Swing Designer > 'JFrame(원하는 항목)' 선택 후 `Next`

    <img src="https://t1.daumcdn.net/cfile/tistory/99B9A5415B35858E33" height=400>
8. `Name에 frame 이름 작성 후 Finish`<br>
Package는 따로 지정하지 않으면 default패키지에 JFrame 클래스가 생성된다.

    <img src="https://t1.daumcdn.net/cfile/tistory/99832F415B35858E02" height=400>
9. **Source 화면** &nbsp;&nbsp;화면 하단에 Design 탭을 클릭하여 GUI 디자인을 편집한다.

    <img src="https://t1.daumcdn.net/cfile/tistory/999F33415B35858F33" height=400>
10. **Design 화면** &nbsp;&nbsp;Palette에서 원하는 항목을 화살표 방향으로 끌어다가 Frame 위에 놓으면 추가된다.

    <img src="https://t1.daumcdn.net/cfile/tistory/992C08415B35858F2D" height=420>
11. 다시 Source 화면으로 돌아가면 추가시킨 항목들에 대한 코드가 추가적으로 작성된 걸 볼 수 있다.

<br>

# 프로젝트
## 기본 구조
<img src="https://user-images.githubusercontent.com/41332126/72410983-30740080-37ad-11ea-888f-70b56d9b359c.png" height=300>

&nbsp;위 사진은 프로젝트의 전체 구조를 보여 주낟. 전체 프로그램의 구조는 MVC 패턴에 따라 설계되며, 클라이언트와 서버 모드 스레드를 기반으로 동작한다. JSON 규격에 맞게 메시지를 전송하며 Gson 파서를 사용하여 객체로 매핑한다. 프로젝트의 결과물은 동시에 여러 클라이언트의 접속이 가능한 채팅 클라이언트와 서버 프로그램이며, 자바의 특성상 윈도우, 맥, 리눅스에서 모두 실행할 수 있다. 특히 UI 부분과 컨트롤러 일부 코드를 수정하면 자바로 개발할 수 있는 안드로이드용 클라이언트 앱도 비교적 쉽게 구현할 수 있다.<br>
> **MVC 패턴** &nbsp;&nbsp;(Model-View-Controller) <br>
&nbsp;MVC 패턴은 소프트웨어를 효과적으로 설계하는 디자인 패턴 중 하나로 대부분의 GUI 프로그램 개발에 적합하다고 열려져 있다. 화면의 구성 요소와 프로그램의 로직, 화면에 표현되는 데이터를 분리해야 하는 필요성이 제기되기 때문이다.
> - Model<br>
뷰에 표현될 데이터를 관리하는 객체<br>컨트롤러를 이용하여 데이터 업데이트를 관리한다.
> - View<br>
화면에 보이는 모든 시각적인 영역을 구현하는 객체<br>뷰 자체가 동적으로 변할 수 있으며 뷰에서 여러 이벤트가 발생한다는 점 때문에 이런 동적인 요소들을 뷰 객체가 아닌 컨트롤러에서 처리할 수 있도록 하는 위임 처리를 해야 한다.
> - Controller<br>
MVC 패턴에 따르는 프로그램의 핵심 부분으로, UI와 데이터의 중계 역할<br>
UI에서 발생하는 이벤트를 처리하고 그에 따른 화면 변화 등을 제어한다.

<br>

## 기능 정의
| 구분 | 기능 |
|:--------|:--------|
| 클라이언트 | 로그인 및 로그아웃 |
| | 대화명 입력 및 표시 |
| | 채팅 메시지 출력 |
| | 프로그램 종료 |
| 서버 | 클라이언트 대기 및 연결 |
| | 다중 클라이언트 채팅 지원 |
| | 연결된 클라이언트 목록 관리 |
| | 채팅 메시지 수신 및 브로드캐스팅 |
| | 로그 출력 |
<br>

## GUI Mock-up
![image](https://user-images.githubusercontent.com/41332126/72704063-f6cf3b00-3b9a-11ea-944e-e5219cdb4e6a.png)

<br>

## JSON 라이브러리 설치
JSON 메시지를 자바 객체로 쉽게 변환할 수 있는 Gson 라이브러리를 사용한다.
1. github.com/google/gson 접속 후 README에서 Downlads 부분의 `Gson jar downloads` 클릭
2. `Downlads 클릭 > jar 선택 > 적당한 폴더에 다운로드`<br>2.3 이상의 최신 버전을 사용하면 되는데 업로드된 파일은 2.3.1 버전을 사용하고있다.

	<img src="https://user-images.githubusercontent.com/41332126/72705710-87a81580-3b9f-11ea-88cc-6d46d1896d7f.png" height=350>
3. 이클립스 > 프로젝트 우클릭 > Properties (Alt+Enter) > Java Build Path > Libraries > `Add External JARs... > gson-2.x.x.jar 파일 선택` > OK<br>라이브러리 목록에 gson이 추가되어 있는지 확인한다.
	
	<img src="https://user-images.githubusercontent.com/41332126/72706891-790f2d80-3ba2-11ea-99bd-404966208743.png" height=400>

<br>

## 클래스 설계 
# MultiChatServer.java
&nbsp;서버 프로그램은 서비스를 제공하느 프로그램을 말하며, 멀티 채팅 서버는 다중 사용자 연결을 관리하고 서로 간의 대화를 중계하는 역할을 한다. <br>여러 클라이언트와 동시에 연결하기 위해 각 클라이언트 연결을 스레드로 처리하며 클라이언트를 종료할 때 해당 스레드도 종료하도록 프로그래밍한다. <br>연결된 클라이언트가 있으면 새로운 스레드 클래스인 ChatThread 클래스를 생성하며, 생성된 인스턴스를 ArrayList인 chatlist에 추가한다.

*&nbsp;원래 클라이언트와 서버는 서로 다른 컴퓨터에서 실행하는 것이 기본이지만 여기서는 편의상 같은 컴퓨터에서 실행하여 테스트한다고 가정한다.<br>&nbsp;&nbsp;클라이언트 코드에서 IP 주소만 변경하면 원격 서버 접속도 가능하다.

| 메서드 | 설명 |
|:--------|:--------|
| MultiChatServer() | 생성자 |
| ↳ void init() | 화면을 구성하는 컴포넌트 초기화 및 레이아웃 배치 등 전반적인 내용 구현 |
| ↳ void addButtonActionListener() | 모든 버튼의 이벤트 핸들러 일괄 등록 메서드 |
| void actionPerformed(ActionEvent e) | 실제 이벤트 핸들러 구현 |

```java
// GUI 부분
import java.awt.Color;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.DefaultListModel;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.border.EmptyBorder;

// 통신 부분
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.HashSet;
import java.util.logging.Logger;
import com.google.gson.Gson;
```
```java
public class MultiChatServer extends JFrame implements ActionListener {
    // GUI 부분
	private JPanel contentPane;
	private JTextArea s_msgOut= new JTextArea(); // server print out
	private JButton startButton = new JButton("서버 실행");
	private JButton stopButton = new JButton("서버 중지");	

    // 통신 부분
    private ServerSocket ss = null; // 서버 소켓
	private Socket s= null; // 클라이언트 연결 소켓  
    ArrayList<ChatThread> chatThreads = new ArrayList<ChatThread>(); // 연결된 클라이언트 스레드 관리
	Logger logger; // 메시지 기록
	
	ArrayList<String> userList = new ArrayList<String>();
	HashSet hs = new HashSet();
	ArrayList<String> tmpList = new ArrayList<String>();
	DefaultListModel<String> user;
	int people = 0;
```
```java	
    // <GUI 부분>
	MultiChatServer() {  // 생성자 
		super("Server"); // the title for the frame
		init();  		 // 화면 구성 메소드
		addButtonActionListener(); // 액션리스너 설정 메소드
	}
	
	private void init() {
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 321, 364);
		setResizable(false); // 창 사이즈 고정

		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		contentPane.setLayout(null);
		setContentPane(contentPane);
		
		JScrollPane scrollPane = new JScrollPane(s_msgOut, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED, JScrollPane.HORIZONTAL_SCROLLBAR_NEVER); // 스크롤 열-필요에 따라, 행-없음
		scrollPane.setBounds(18, 15, 281, 256);
		contentPane.add(scrollPane);
		
		s_msgOut.setFont(new Font("a시네마M", Font.PLAIN, 13));
		s_msgOut.setForeground(Color.WHITE);
		s_msgOut.setBackground(new Color(153, 153, 102));
		s_msgOut.setLineWrap(true); // 줄바꿈 yes
		s_msgOut.setWrapStyleWord(true); // 단어 단위 줄바꿈 yes
		s_msgOut.append(" ~ 서버 실행중 ~ \n");
		scrollPane.setViewportView(s_msgOut);
		
		startButton.setBounds(28, 285, 115, 33);
		startButton.setFont(new Font("a시네마M", Font.PLAIN, 14));
		contentPane.add(startButton);
		
		stopButton.setBounds(173, 285, 115, 33);
		stopButton.setFont(new Font("a시네마M", Font.PLAIN, 14));
		contentPane.add(stopButton);
		
		setVisible(true); // true=show, false=hide
	}
	
	private void addButtonActionListener() {
		startButton.addActionListener(this);
		stopButton.addActionListener(this);
	}
	
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		
		if(obj == startButton) {
			System.out.println("서버 실행 버튼 클릭");
			start();  // 소켓 생성 및 사용자 접속 대기
		}
		else if(obj == stopButton) {
			System.out.println("서버 중지 버튼 클릭");
			System.exit(0);
		}
	}
    // </GUI 부분>
```
<br>

| 메서드 | 설명 |
|:--------|:--------|
| void start() | 서버의 메인 실행 메서드. ServerSocket을 생성하고 클라이언트 연결 및 스레드 생성 처리 |
| msgSendAll(String msg) | 서버가 수신한 메시지를 연결된 모든 클라이언트에 전송 |
| msgSendWho(String msg, int send, int rcv) | 서버가 수신한 메시지를 연결된 특정 클라이언트에 전송 |
| class ChatThread | 각 클라이언트와 연결 유지, 메시지 송수신 담당 |
| main(String[] args) | 실행 메인 메서드 |

```java
    // <통신 부분>
    public void start() {
		logger = Logger.getLogger(this.getClass().getName());
		
		try {
			ss = new ServerSocket(12345);  // 12345 포트 사용
			logger.info("MultiChatServer start");
			
			while (true) {
				s = ss.accept();
				
				ChatThread chat = new ChatThread();
				chatThreads.add(chat);
				chat.start();
			}
		}
		catch (Exception e) {
			logger.info("[MultiChatServer]start() Exception 발생!!");
			e.printStackTrace();
		}
	}
```
```java	
	void msgSendAll(String msg) { // 연결된 모든 클라이언트에 메시지 중계
		for(ChatThread ct : chatThreads) {
			ct.outMsg.println(msg);
		}
	}

	void msgSendWho(String msg, int send, int rcv) {
		ChatThread ct = chatThreads.get(rcv);
		ct.outMsg.println(msg);
		ct = chatThreads.get(send);
		ct.outMsg.println(msg);
	}
```
↓ Thread 클래스를 상속해서 구현하고, MultiChatServer클래스와 더 쉽게 연결하여 사용할 수 있도록 내부 클래스 형태로 구현한다.
```java
	// 각각의 클라이언트 관리를 위한 쓰레드 클래스
	// 1개의 스레드에서는 1개의 일만 처리할 수 있다
	class ChatThread extends Thread {
		String msg; // 수신 메시지 및 파싱 메시지 처리를 위한 변수 선언
		Message m = new Message(); // 메시지 객체 생성
		Gson gson = new Gson(); // Json Parser 초기화

		// 입출력 스트림
		private BufferedReader inMsg = null;
		private PrintWriter outMsg = null;

		public void run() { // 스레드에서 처리할 일을 기재한다
			boolean status = true;
			logger.info("ChatThread start...");
			
			try {
				// 입출력 스트림 생성
				inMsg = new BufferedReader(new InputStreamReader(s.getInputStream()));
				outMsg = new PrintWriter(s.getOutputStream(),true);
				
				// 상태정보가 true 이면 루프를 돌면서 사용자로 부터 수신된 메시지 처리
				while(status) {
					msg = inMsg.readLine(); // 수신된 메시지를 msg 변수에 저장					
					m = gson.fromJson(msg,Message.class); // JSON 메시지를 Message 객체로 매핑
												
					// 파싱된 문자열 배열의 두번째 요소 값에 따라 처리
					if(m.getType().equals("logout")) { // 로그아웃 메시지인 경우
						tmpList.remove(m.getId());
						user = new DefaultListModel<>();
						for(int i=0; i<tmpList.size(); i++) {
							user.add(i, tmpList.get(i));
						}
						
						userList.remove(m.getId());
						hs.remove(m.getId());
						chatThreads.remove(this);
						
						people = userList.size();
						
						s_msgOut.append("사용자 "+m.getId()+" 로그아웃했습니다.\n");
						msgSendAll(gson.toJson(new Message(m.getId(),"","님이 로그아웃했습니다.","server", "all", user, people)));
						status = false; // 해당 클라이언트 스레드 종료로 인해 status 를 false 로 설정
					}
					else if(m.getType().equals("login")) { // 로그인 메시지인 경우
						tmpList.add(people, m.getId());
						user = new DefaultListModel<>();
						for(int i=0; i<tmpList.size(); i++) {
							user.add(i, tmpList.get(i));
						}  
						
						userList.add(m.getId());
						hs.addAll(userList);
						userList.clear();
						userList.addAll(hs);
						
						people = userList.size();
						
						msgSendAll(gson.toJson(new Message(m.getId(),"","님이 로그인했습니다.","server", "all", user, people)));
						System.out.println(msg);
						s_msgOut.append("사용자 "+m.getId()+" 로그인했습니다.\n");
					}
					else if(m.getType().equals("secret")) { // 귓속말 메시지인 경우
						int numS = user.indexOf(m.getId());
						int numR = user.indexOf(m.getRcvid());
						//System.out.println("numS : "+numS+"\tnumR : "+numR);

						msgSendWho(gson.toJson(new Message(m.getId(),"", m.getMsg(),"s_secret", m.getRcvid(), user, people)), numS, numR);
						//System.out.println(msg);
						s_msgOut.append("(귓속말)" + m.getId() + "→" + m.getRcvid() + " : " + m.getMsg()+"\n");
					}
					else { // 그밖의 경우, 일반 메시지인 경우
						msgSendAll(msg);
						s_msgOut.append(m.getId() + " : " + m.getMsg() + "\n");
					}
				} // .while
				// 루프를 벗어나면 클라이언트 연결 종료 이므로 스레드 인터럽트
				this.interrupt();
				logger.info(this.getName() + " 종료됨!!");
			} // .try
			catch (Exception e) {
				chatThreads.remove(this);
				logger.info("[ChatThread]run() IOException 발생!!");
                e.printStackTrace();
			}
		} // .run
	}
```
```java	
	public static void main(String[] args) {
		MultiChatServer server = new MultiChatServer();
		server.start();
	}
    // </통신 부분>
} // .MultiChatServer class
```
<br>

# MultiChatData.java (Model class)
&nbsp;화면에 필요한 데이터를 제공하고 업데이트하는 기능을 제공한다.<br>
프로그램의 기능이 확장되어 내용이 복잡해지면 UI 변경이 발생하는 상황에 따라 별도의 메서드를 추가로 만들어도 된다.

| 메서드 | 설명 |
|:--------|:--------|
| addObj(JComponent comp) | 데이터 변경시 업데이트할 UI 컴포넌트(범용적 타입) 등록 |
| void refreshData(String msg) | 파라미터로 전달된 메시지 내용으로 UI 데이터 업데이트 |

```java
import javax.swing.JComponent;
import javax.swing.JTextArea;

public class MultiChatData {
    JTextArea msgOut;

    public void addObj(JComponent comp) {
        this.msgOut = (JTextArea)comp;
    }

    public void refreshData(String msg) {
        msgOut.append(msg); // 채팅 메시지 창의 텍스트 추가
    }
}
```
<br>

# MultiChatUI.java (View class)
&nbsp;화면의 구성 요소를 정의하고 레이웃을 이용하여 컴포넌트를 배치한다. <br>또 MultiChatUI 클래스에서는 다른 클래스들을 생성하거나 참조하지 않는다.<br>
이벤트를 발생할 때 처리할 핸들러 클래스는 파라미터로 전달하는데 <br>컨트롤러에서 메서드를 호출하기 때문에 실제 이벤트 핸들러 코드는 컨트롤러의 appMain() 메서드에서 구현한다. 

| 메서드 | 설명 |
|:--------|:--------|
| MultiChatUI() | 생성자 |
| ↳ void init() | 화면을 구성하는 컴포넌트 초기화 및 레이아웃 배치 등 전반적인 내용 구현 |
| void addButtonActionListener(ActionListener listener) | 모든 버튼의 이벤트 핸들러 일괄 등록 메서드 |
| void addEnterKeyListener(KeyListener listener) | 모든 엔터키의 이벤트 핸들러 일괄 등록 메서드 |
| void addButtonWindowListenr(WindowListener listener) | 모든 창의 이벤트 핸들러 일괄 등록 메서드 |

```java
import java.awt.CardLayout;
import java.awt.Color;
import java.awt.Container;
import java.awt.Font;
import java.awt.event.ActionListener;
import java.awt.event.KeyListener;
import java.awt.event.WindowListener;

import javax.swing.DefaultListModel;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JList;
import javax.swing.JPanel;
import javax.swing.JRadioButton;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.ListSelectionModel;
import javax.swing.border.EmptyBorder;

public class MultiChatUI extends JFrame {
	/*
	* 기본적으로 private을 사용하여 멤버 변수를 선언하되
	* 컨트롤러에서 UI를 핸들링하는 데 필요한 객체들은 protected로 선언한다.
	*/

	// 전체
	private JPanel contentPane;
	
	// 채팅
	private JLabel chatLabel;
	protected JTextArea c_msgOut; // client print out
	protected JTextField msgInput; // enter the txt
	protected JButton sendButton;
	
	// 로그인/로그아웃
	protected static String id; 
	
	private JLabel userLabel;
	private JPanel loginPanel; // = idInput + loginButton
	protected JTextField idInput; // enter the id
	protected JButton loginButton;
	private JPanel logoutPanel; // = idOutLabel + logoutButton
	protected JLabel idOutLabel;
	protected JButton logoutButton;
	protected Container tab; // = loginPanel + logoutPanel
	protected CardLayout cardLayout; // 필요에 따라 패널을 바꿔서 보여준다
	
	// 접속자
	protected JLabel contactLabel;
	protected DefaultListModel<String> nameOutModel; // membership list
	protected JList<String> nameOut;

	// 귓속말
	protected JRadioButton secretRadio; // whisper or not
	// 기록 삭제
	protected JButton deleteButton;
	// 종료
	protected JButton exitButton; // sign out and close the window
	
	public MultiChatUI() {
		super(" 멀티챗 ☆.。.:*・°☆.。.:*・°☆.。.:*・°☆.。.:*・°☆");
		init();
	}
	
	private void init() {
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 510, 378);
		setResizable(false);

		contentPane = new JPanel();
		contentPane.setBackground(new Color(230, 230, 250));
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		contentPane.setLayout(null);
		setContentPane(contentPane);

		chatLabel = new JLabel(" ~ 채팅방 ~");
		chatLabel.setFont(new Font("a시네마M", Font.PLAIN, 13));
		chatLabel.setBounds(12, 3, 77, 25);
		contentPane.add(chatLabel);
		
		/*
		* JTextArea 자체는 스크롤 기능을 제공하지 않으므로
		* JScrollPane을 컨테이너로 사용하여 JTextAre를 배치해야 한다.
		*/
		JScrollPane scrollPane1 = new JScrollPane(c_msgOut, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED, JScrollPane.HORIZONTAL_SCROLLBAR_NEVER); // 스크롤 열-필요에 따라, 행-없음
		scrollPane1.setBounds(12, 27, 338, 273);
		contentPane.add(scrollPane1);
		
		c_msgOut = new JTextArea();
		c_msgOut.setFont(new Font("a시네마M", Font.PLAIN, 12));
		c_msgOut.setBackground(new Color(250, 235, 215));
		c_msgOut.setEditable(false); // 출력 전용. 내용 수정 불가 
		c_msgOut.setLineWrap(true); // 줄바꿈 yes
		c_msgOut.setWrapStyleWord(false); // 단어 단위 줄바꿈 no
		scrollPane1.setViewportView(c_msgOut);
		
		msgInput = new JTextField();
		msgInput.setFont(new Font("a시네마M", Font.PLAIN, 12));
		msgInput.setBounds(12, 313, 277, 21);
		msgInput.setColumns(10);
		contentPane.add(msgInput);
		
		sendButton = new JButton("전송");
		sendButton.setFont(new Font("a시네마M", Font.PLAIN, 12));
		sendButton.setBounds(289, 312, 61, 23);
		contentPane.add(sendButton);
		
		userLabel = new JLabel(" ~ 사용자 ~\r\n");
		userLabel.setFont(new Font("a시네마M", Font.PLAIN, 13));
		userLabel.setBounds(374, 10, 116, 15);
		contentPane.add(userLabel);

		loginPanel = new JPanel();
		loginPanel.setBackground(new Color(230, 230, 250));
		loginPanel.setBounds(374, 28, 116, 46);
		loginPanel.setLayout(null);
		
		idInput = new JTextField();
		idInput.setFont(new Font("a시네마M", Font.PLAIN, 12));
		idInput.setBounds(0, 0, 116, 21);
		idInput.setColumns(10);
		loginPanel.add(idInput);
		
		loginButton = new JButton("로그인");
		loginButton.setFont(new Font("a시네마M", Font.PLAIN, 12));
		loginButton.setBounds(0, 23, 116, 23);
		loginPanel.add(loginButton);
		
		logoutPanel = new JPanel();
		logoutPanel.setBackground(new Color(230, 230, 250));
		logoutPanel.setBounds(374, 28, 116, 46);
		logoutPanel.setLayout(null);

		idOutLabel = new JLabel();
		idOutLabel.setFont(new Font("a시네마M", Font.PLAIN, 12));
		idOutLabel.setBounds(0, 0, 116, 21);
		logoutPanel.add(idOutLabel);
		
		logoutButton = new JButton("로그아웃");
		logoutButton.setFont(new Font("a시네마M", Font.PLAIN, 12));
		logoutButton.setBounds(0, 23, 116, 23);
		logoutPanel.add(logoutButton);

		tab = new JPanel();
		cardLayout = new CardLayout(0, 0);
		tab.setLayout(cardLayout);
		tab.setBounds(374, 28, 116, 46);
		tab.add(loginPanel, "login");
		tab.add(logoutPanel, "logout");
		add(tab);
		cardLayout.show(tab, "login");
		
		contactLabel = new JLabel(" ~ 접속자 ~");
		contactLabel.setFont(new Font("a시네마M", Font.PLAIN, 13));
		contactLabel.setBounds(374, 96, 77, 15);
		contentPane.add(contactLabel);
		
		JScrollPane scrollpane2 = new JScrollPane(nameOut, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED, JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED); // 스크롤 열-필요에 따라, 행-필요에 따라
		scrollpane2.setBounds(374, 114, 116, 120);
		contentPane.add(scrollpane2);
		
		nameOutModel = new DefaultListModel<>();
		nameOut = new JList<>(nameOutModel);
		nameOut.setFont(new Font("a시네마M", Font.PLAIN, 12));
		nameOut.setSelectionMode(ListSelectionModel.SINGLE_SELECTION); // select one list index at a time
		scrollpane2.setViewportView(nameOut);
		
		secretRadio = new JRadioButton("귓속말");
		secretRadio.setFont(new Font("a시네마M", Font.PLAIN, 12));
		secretRadio.setBackground(new Color(230, 230, 250));
		secretRadio.setBounds(374, 240, 116, 23);
		contentPane.add(secretRadio);

		deleteButton = new JButton("전체 기록 삭제");
		deleteButton.setFont(new Font("a시네마M", Font.PLAIN, 12));
		deleteButton.setBounds(374, 282, 116, 23);
		contentPane.add(deleteButton);
		
		exitButton = new JButton("종료");
		exitButton.setFont(new Font("a시네마M", Font.PLAIN, 12));
		exitButton.setBounds(374, 311, 116, 23);
		contentPane.add(exitButton);

		setVisible(true);
	}
	
	/*
	* MultiChatUI - 이벤트 등록
	* MultiChatController - 이벤트 처리 부분 구현
	* 파라미터로 전달된 리스너 객체를 핸들러 클래스로 사용하도록 지정하여
	* 메서드를 호출하는 쪽에서 이벤트 코드를 구현할 수 있도록 한다.
	*/
	public void addButtonActionListener(ActionListener listener) {
		sendButton.addActionListener(listener);
		loginButton.addActionListener(listener);
		logoutButton.addActionListener(listener);
		deleteButton.addActionListener(listener);
		exitButton.addActionListener(listener);
	}
	
	public void addEnterKeyListener(KeyListener listener) {
		msgInput.addKeyListener(listener);
		sendButton.addKeyListener(listener);
		idInput.addKeyListener(listener);
		loginButton.addKeyListener(listener);
		logoutButton.addKeyListener(listener);
		deleteButton.addKeyListener(listener);
		exitButton.addKeyListener(listener);
	}
	
	public void addButtonWindowListenr(WindowListener listener) {
		this.addWindowListener(listener);
	}

}
```
<br>

# MultiChatController.java (Controller class)
&nbsp;프로그램의 메인으로 서버와 통신을 담당하고, 클라이언트에서 발생하는 이벤트에서 발생하는 <br>이벤트에 따라 UI를 변경하거나 수신된 메시지를 출력하는 등 프로그램의 전반적인 로직을 처리한다. <br>view 클래스인 MultiChatUI와 독립적으로 구현하며, 서버와 통신은 별도 스레드로 다룬다. 

| 메서드 | 설명 |
|:--------|:--------|
| MultiChatController(MultiChatData chatData, MultiChatUI v) | 모델과 뷰 객체를 파라미터로 하는 생성자 |
| void appMain() | 컨트롤러 클래스의 메인 로직. UI에서 발생한 이벤트를 위임받아 처리 |

```java
import static java.util.logging.Level.INFO;
import static java.util.logging.Level.WARNING;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;
import javax.swing.DefaultListModel;
import javax.swing.JOptionPane;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;
import java.util.logging.Logger;

import com.google.gson.Gson;

public class MultiChatController implements Runnable {
	// 뷰 클래스 참조 객체
	private final MultiChatUI v;
	
	// 데이터 클래스 참조 객체
	private final MultiChatData chatData;
	
	// 소켓 연결을 위한 변수 선언
	private String ip = "127.0.0.1";
	private Socket socket;
	private BufferedReader inMsg = null;
	private PrintWriter outMsg = null;
	
	// 메시지 파싱을 위한 객체 생성
	Gson gson = new Gson();
	Message m;
	 
	boolean status; // 상태 플래그
	Logger logger; // 로거 객체 
	Thread thread; // 메시지 수신 스레드
	
	// 메시지 전송 시간, 날짜 객체 생성
	SimpleDateFormat time_sdf = new SimpleDateFormat("a hh:mm");
	Date date = new Date();
	SimpleDateFormat date_sdf = new SimpleDateFormat("yyyy년 MMM dd일 EEE요일", Locale.KOREA);
	int count = 0;
	
	// 접속자
	DefaultListModel<String> check = new DefaultListModel<>();
	int people;
```
```java	
	// 모델과 뷰 객체를 파라미터로 하는 생성자
	public MultiChatController(MultiChatData chatData, MultiChatUI v) {
		// 로거 객체 초기화
		logger = Logger.getLogger(this.getClass().getName());
		
		// 모델과 뷰 클래스 참조
		this.chatData = chatData;
		this.v = v;
	}
```
```java	
	// 어플리케이션 메인 실행 메서드
	public void appMain()
	{
		// 데이터 객체에서 데이터 변화를 처리할 UI 객체 추가
		chatData.addObj(v.c_msgOut);
		
		// <엔터키 이벤트>
		v.addEnterKeyListener(new KeyListener()
		{
			@Override
			public void keyPressed(KeyEvent e) {
				int key = e.getKeyCode();
				if(key == KeyEvent.VK_ENTER) {
					Object obj = e.getSource();
					if (obj == v.idInput || obj == v.loginButton)
					{
						v.id = v.idInput.getText();
						v.idOutLabel.setText("user : " +v.id);
						v.cardLayout.show(v.tab, "logout");
						connectServer();
					}
					else if (obj == v.logoutButton) {
						//로그아웃 메시지 전송
						outMsg.println(gson.toJson(new Message(v.id, "", "", "logout", "all", v.nameOutModel, 0)));
						//대화창 클리어
						v.c_msgOut.setText("");
						//접속자리스트 지우기
						v.nameOutModel.removeAllElements();
						v.nameOut.setModel(v.nameOutModel);
						v.idInput.setText("");
						//로그인 패널로 전환
						v.cardLayout.show(v.tab, "login");
						outMsg.close();
						
						try {
							inMsg.close();
							socket.close();
						}
						catch (IOException ex) {
							ex.printStackTrace();
						}
						status = false;
					}
					else if (obj == v.msgInput || obj == v.sendButton) {
						if(v.secretRadio.isSelected() == true) {
							String rcvid = v.nameOut.getSelectedValue();
							if(rcvid == null) {
								JOptionPane.showMessageDialog(v.getContentPane(), "사람을 선택해주세요!");
							}
							else {
								JOptionPane.showMessageDialog(v.getContentPane(), rcvid+"님에게 귓속말을 보내시겠습니까?");
								outMsg.println(gson.toJson(new Message(v.id, "", v.msgInput.getText(), "secret", rcvid, v.nameOutModel, 0)));
			                    v.msgInput.setText("");
							}
						}
						else {
							// 메시지 전송
		                    outMsg.println(gson.toJson(new Message(v.id, "", v.msgInput.getText(), "msg", "all", v.nameOutModel, 0)));
		                    // 입력창 클리어
		                    v.msgInput.setText("");
						}
					}
					else if (obj == v.exitButton) {
						v.nameOutModel.removeAllElements();
						v.nameOut.setModel(v.nameOutModel);
						outMsg.println(gson.toJson(new Message(v.id, "", "", "logout", "all", v.nameOutModel, 0)));
						System.exit(0);
					}
					else if (obj == v.deleteButton) {
						outMsg.flush();
						v.c_msgOut.setText("--------------------------------- 기록 삭제 --------------------------------\n");
					}
					
				}
			}

			@Override
			public void keyReleased(KeyEvent e) {}
			@Override
			public void keyTyped(KeyEvent e) {}
		});
		// </엔터키 이벤트>

		// <버튼 이벤트>
		v.addButtonActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				Object obj = e.getSource();
				if (obj == v.loginButton) {
					v.id = v.idInput.getText();
					v.idOutLabel.setText("user : " +v.id);
					v.cardLayout.show(v.tab, "logout");
					connectServer();
				}
				else if (obj == v.logoutButton) {
					outMsg.println(gson.toJson(new Message(v.id, "", "", "logout", "all", v.nameOutModel, 0)));
					v.c_msgOut.setText("");
					v.contactLabel.setText(" ~ 접속자 ~");
					v.nameOutModel.removeAllElements();
					v.nameOut.setModel(v.nameOutModel);
					v.idInput.setText("");
					v.cardLayout.show(v.tab, "login");
					outMsg.close();
					
					try {
						inMsg.close();
						socket.close();
					}
					catch (IOException ex) {
						ex.printStackTrace();
					}
					status = false;
				}
				else if (obj == v.sendButton) {
					if(v.secretRadio.isSelected() == true) {
						String rcvid = v.nameOut.getSelectedValue();
						if(rcvid == null) {
							JOptionPane.showMessageDialog(v.getContentPane(), "사람을 선택해주세요!");
						}
						else {
							JOptionPane.showMessageDialog(v.getContentPane(), rcvid+"님에게 귓속말을 보내시겠습니까?");
							outMsg.println(gson.toJson(new Message(v.id, "", v.msgInput.getText(), "secret", rcvid, v.nameOutModel, 0)));
		                    v.msgInput.setText("");
						}
					}
					else {
	                    outMsg.println(gson.toJson(new Message(v.id, "", v.msgInput.getText(), "msg", "all", v.nameOutModel, 0)));
	                    v.msgInput.setText("");
					}
				}
				else if (obj == v.exitButton) {
					v.nameOutModel.removeAllElements();
					v.nameOut.setModel(v.nameOutModel);
					outMsg.println(gson.toJson(new Message(v.id, "", "", "logout", "all", v.nameOutModel, 0)));
					System.exit(0);
				}
				else if (obj == v.deleteButton) {
					outMsg.flush();
					v.c_msgOut.setText("--------------------------------- 기록 삭제 --------------------------------\n");
				}
			}
		});	
		// </버튼 이벤트>

		// <창 이벤트>
		v.addButtonWindowListenr(new WindowListener() {
			@Override
			public void windowClosing(WindowEvent e) {
				v.nameOutModel.removeAllElements();
				v.nameOut.setModel(v.nameOutModel);
				outMsg.println(gson.toJson(new Message(v.id, "", "", "logout", "all", v.nameOutModel, 0)));
				System.exit(0);			
			}
			        			
			@Override
			public void windowActivated(WindowEvent e) {}
			@Override
			public void windowClosed(WindowEvent e) {}
			@Override
			public void windowDeactivated(WindowEvent e) {}
			@Override
			public void windowDeiconified(WindowEvent e) {}
			@Override
			public void windowIconified(WindowEvent e) {}
			@Override
			public void windowOpened(WindowEvent e) {}
		});
		// </창 이벤트>
	}
```
<br>

| 메서드 | 설명 |
|:--------|:--------|
| void connectServer() | 채팅 서버 접속 위한 메서드 |
| void run() | 서버 연결 후 메시지 수신을 UI 동작과 상관없이 독립적으로 처리하는 스레드를 실행 |
| main(String[] args) | 실행 메인 메서드 |
↓ [로그인] 버튼을 눌렀을 대 호출되는 메서드. 서버와 연결하고 입출력 스트림을 만든 후 메시지 수신에 필요한 스레드를 생성한다. 

```java
	// 서버 접속을 위한 메서드 
	public void connectServer() {
		try {
			// 소켓 생성
			socket = new Socket(ip , 12345);
			logger.log(INFO, "[Client]Sever 연결 성공!!");
			
			// 입출력 스트림 생성
			inMsg = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			outMsg = new PrintWriter(socket.getOutputStream(),true);
			
			// 서버에 로그인 메시지 전달
			m = new Message(v.id, "", "", "login", "all", v.nameOutModel, 0);
			outMsg.println(gson.toJson(m));
			
			// 메시지 수신을 위한 스레드 생성
			thread = new Thread(this);
			thread.start();
		}
		catch(Exception e) {
			logger.log(WARNING, "[MultiChatUI]connectServer() Exception 발생!!");
			e.printStackTrace();
		}
	}
```
↓ connectServer()에서 thread.start() 메서드를 실행하면서 호출된다.<br>&nbsp;&nbsp;UI 실행과 독립적으로 서버와 네트워크 연결을 유지하며, 수신된 메시지를 처리하는 역할을 담당한다.<br>&nbsp;&nbsp;일반적인 소켓 클라이언트 프로그램과 마찬가지로 while() 문을 반복하면서 <br>&nbsp;&nbsp;서버에서 전송하는 메시지를 행 단위로 읽어 와 JSON 메시지를 Message 객체로 변환한다.<br>&nbsp;&nbsp;그리고 데이터 클래스인 MultiChatData의 refresh() 메서드를 호출하여 변경할 메시지를 전달한다.
```java	
    // 메시지 수신을 독립적으로 처리하기 위한 스레드 실행
	@Override
    public void run() {
        // 수신 메시지 처리를 위한 변수
        String msg;
        
        if(count == 0) { // 오늘 첫 채팅시에만 날짜 추가
        	chatData.refreshData("----------------------- "+date_sdf.format(date)+" -----------------------\n");
        	count++;
        }

        this.status=true;
        while(status) {
            try {
                // 메시지 수신 및 파싱
                msg = inMsg.readLine();
                m = gson.fromJson(msg, Message.class);
                
                Date date = new Date();
                
                // 접속자들
                check = m.getCheck();
                people = m.getPeople();

                // MultiChatData 객체를 통해 데이터 갱신
                if(m.getType().equals("server")) {
                	v.nameOut.setModel(check);
                	v.contactLabel.setText("접속자   "+people+"명");
                	// 로그인/로그아웃 메시지 출력
                	chatData.refreshData(m.getId() + "" + m.getMsg() +"\n");
                }
                else if(m.getType().equals("s_secret")) {
                	chatData.refreshData(m.getId() + "→" + m.getRcvid() + " : " + m.getMsg() +"               "+time_sdf.format(date) +"\n");
                }
                else {
                	// 일반 메시지 출력
                	chatData.refreshData(m.getId() + " : " + m.getMsg() +"               "+time_sdf.format(date) +"\n");
                }

                // 커서를 현재 대화 메시지에 보여줌
                v.c_msgOut.setCaretPosition(v.c_msgOut.getDocument().getLength());
            }
            catch(IOException e) {
                logger.log(WARNING,"[MultiChatUI]메시지 스트림 종료!!");
            }
        }
        logger.info("[MultiChatUI]" + thread.getName()+ " 메시지 수신 스레드 종료됨!!");
    }
```
```java	
	public static void main(String[] args)
	{
		MultiChatController app = new MultiChatController(new MultiChatData(), new MultiChatUI());
		app.appMain();
	}

} // .MultiChatController class
```
<br>

# Message.java
&nbsp;클라이언트와 서버 간의 통신에 사용하는 JSON 규격의 메시지를 자바 객체로 변환하여 더 쉽게 사용하게 해준다.<br>현재 JSON(Javascript Object Notation)은 인터넷으로 시스템이나 프로그램 간에 데이터를 주고받는 메시지 규격으로 널리 사용된다.<br>이 메신저에서는 구글에서 만든, JSON 메시지를 자바 객체로 변환하기에 편리한 파서인 Gson을 사용한다.

| 번호 | 필드(key) | 설명(value) |
|:--------|:--------|:--------|
| 1 | id | 사용자 아이디 |
| 2 | passwd | 비밀번호 |
| 3 | msg | 전달 메시지 |
| 4 | type | 메시지 유형(login, logout, msg, secret) |
| 5 | rcvid | 수신자 아이디 |
| 6 | check | 접속자 리스트 |
| 7 | people | 접속자 인원 |

JSON ex) {"id" : "Lilly", "passwd" : "1234", "msg" : "Hello!", "type" : "msg", "rcvid" : "", "check" : DefaultListModelName, people : 3}

```java
import javax.swing.DefaultListModel;

public class Message {
	private String id;		// 아이디
	private String passwd;	// 비밀번호
	private String msg;		// 채팅 메시지
	private String type;	// 메시지 유형(로그인, 로그아웃, 메시지, 귓속말 전달)
	private String rcvid;	// 귓속말 대상
	private DefaultListModel<String> check; // 접속자 리스트
	private int people;		// 접속자 수
	
	public Message(){}
	
	public Message(String id, String passwd, String msg, String type, String rcvid, DefaultListModel<String> check, int people) {
		this.id = id;
		this.passwd = passwd;
		this.msg = msg;
		this.type = type;
		this.rcvid = rcvid;
		this.check = check;
		this.people = people;
	}
	
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	
	public String getPasswd() {
		return passwd;
	}
	public void setPassword(String passwd) {
		this.passwd = passwd;
	}
	
	public String getMsg() {
		return msg;
	}
	public void setMsg(String msg) {
		this.msg = msg;
	}
	
	public String getType() {
		return type;
	}
	public void setType(String type) {
		this.type = type;
	}
	
	public String getRcvid() {
		return rcvid;
	}
	public void setRcvid(String rcvid) {
		this.rcvid = rcvid;
	}
	
	public DefaultListModel<String> getCheck() {
		return check;
	}
	public void setCheck(DefaultListModel<String> check) {
		this.check = check;
	}
	
	public int getPeople() {
		return people;
	}
	public void setPeople(int people) {
		this.people = people;
	}
}
```
<br>

# 실행 환경
### 단일 컴퓨터에서 실행하는 경우
&nbsp;가장 기본적인 형태로, 제한된 여건상 하나의 컴퓨터에서 클라이언트와 서버를 모두 실행하는 경우이다. <br>채팅 프로그램이기 때문에 최소한 두 개의 클라이언트를 실행해야 채팅 테스트를 할 수 있다.

### 여러 컴퓨터에서 실행하는 경우
&nbsp;여건만 된다면 단일 컴퓨터에서 실행하는 것보다 좋은 방법이다. 원래 채팅 프로그램은 원격 서버 컴퓨터와 인터넷에 연결된 <br>여러 클라이언트 컴퓨터가 통신하는 것이므로 최소 세 대의 컴퓨터가 있어야 제대로 테스트할 수 있다. 이때 하나는 서버로 실행하고 <br>나머지는 클라이언트로 실행한다. 서버의 IP 주소가 필요하며, MultiChatController 클래스의 String ip 변수값을 서버 IP로 변경해야 한다.

<br>

# 실행 결과
3인 채팅(첸백시 EXO-CBX ver.)

![결과1](https://user-images.githubusercontent.com/41332126/72777259-62c3a900-3c58-11ea-89f4-f6faca8e127a.gif)

<br>

5인 채팅(레드벨벳 RedVelvet ver.) &nbsp;&nbsp;&nbsp;`썸네일 클릭하면 유튜브로 이동`

[![결과2](https://img.youtube.com/vi/cFraCrZCjkM/0.jpg)](https://www.youtube.com/watch?v=cFraCrZCjkM)