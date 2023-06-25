# #2 PRACTICE PROJECT

### **#2.0 Patterns**

**Layout**

<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

레이아웃을 설정해 \_app 파일에 적용하면 통일된 디자인 패턴을 적용할 수 있다.



**Header**

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

헤더를 설정해 타이틀을 설정할 수 있다.

<figure><img src="../../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

파일별로 지정할 필요없이 따로 컴포넌트를 만들어 Layout에 적용해주면 편하다.



### **#2.1 Fetching Data**

**Image**

public의 이미지를 사용하려면 \<img src="/vercel.svg" /> 와 같이 사용함



**API Keys**

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

react와 같음

값이 없을때 로딩하는 로직을 다시 보자!



### **#2.2 Redirect and Rewrite**

**Redirect**

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

특정 url 방문한 경우 원하는 url로 리다이렉트 시킬 수 있음

**Rewrite**

![](<../../../.gitbook/assets/image (71).png>)![](<../../../.gitbook/assets/image (97).png>)

URL 변환 없이 내용을 변환함

API 주소와 같이 비밀로 할 내용을 숨길 수 있음



### **#2.3 Server Side Rendering**

**Get Server Side Props**

서버 사이드 렌더링 ⇒ 서버에서 api 호출을 진행하고, 그 결과를 props로 내려줌 ⇒ loading 없이 html을 pre-render 할 수 있음 ⇒ 프론트에서 보이지 않음

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>



### **#2.4 Recap**

위 내용 복습



### **#2.5 Dynamic Routes**

/movies/all

위 url이 필요하다면 movies 폴더를 만들고 all.js 파일을 추가한다.

위 상황에서

/movies

위 url이 필요하다면 index.js를 추가한다.

<figure><img src="../../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

/movies/1

위와 같이 동적 루트를 만들고 싶다면 \[변수명].js 파일을 추가한다.



### **#2.6 Movie Detail**

**onClick 이나 Link 로 이동할 URL을 설정 할 수 있음**

<figure><img src="../../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

as 기능 && rewrites 설정 추가 ⇒ url 을 가릴 수 있음

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

url에 정보를 숨켜 props로 내림, url에는 제목이 포함되어 있지만 유저들은 확인할 수 없음



### **#2.7 Catch All**

**CathAllURL**

![](<../../../.gitbook/assets/image (64).png>)

다양한 url 정보를 추가할때 파일명 변경.

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

영화 제목과 id를 url에 추가하고, 이 정보를 활용해 서버에서 렌더링 작업을 할 수 있음 ⇒ SEO 효과적

아래 사진의 상단 부분은 client에서 작업하는 부분이다. 그래서 페이지 이동시 api로 데이터를 받아오는 과정이 필요하기에 loading이 필요하다. 하단은 서버에서 정보를 얻고 렌더링할때 사용하는 부분이다. 따라서 loading 없이 렌더링할 수 있다.

<figure><img src="../../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>



### **#2.8 404 Pages**

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

pages 폴더에 404.js 를 만들고 404페이지에서 보여줄 내용을 추가한다.
