# kakao_map_plugin

[![pub package](https://img.shields.io/pub/v/kakao_map_plugin.svg?color=4285F4)](https://pub.dev/packages/kakao_map_plugin)

**[카카오 지도](https://apis.map.kakao.com/web/guide)** 를 구동할 수 있는 Flutter 플러그인 입니다.

네이티브 라이브러리를 사용한 것이 아닌 Javascript 라이브러리를 이용하여 제작한 플러그인 입니다.

`webview_flutter` package 를 사용하고 있어서 Android, iOS 최소 버전 확인이 필요합니다.

|             | Android        | iOS  |
|-------------|----------------|------|
| **Support** | SDK 19+ or 20+ | 9.0+ |

---

## 시작하기

### 공통

**[카카오 개발자센터](https://developers.kakao.com/)** 에서 javascript key 를 발급받아야 합니다.

`pubspec.yaml`에 dependencies에 작성

``` yaml
dependencies:
  kakao_map_plugin: [최신버전]
```

1. javascript key 등록

* Singleton 으로 되어 있어서 KakaoMap 위젯이 호출 되기 전에만 initialize 하면 됩니다. 여기서는 main 함수에서 호출하도록 했습니다.
* example 에서는 flutter_dotenv 라이브러리를 사용하였습니다. 바로 실행해 보시려면 `example/assets/env/.env.sample`을
  복사하여 `example/assets/env/.env`로 만들어주시고 `.env` 파일 내부에 `APP_KEY=`뒤에 본인의 javascript key 를 넣으시면 됩니다.
* 키워드로 장소검색하기, 카테고리로 장소 검색, 주소로 장소 표시, 좌표로 주소를 얻어오기, 좌표 변환하기 와 같은 services 기능을 사용하려면 baseUrl 을 추가 해야 합니다.
* `.env`에 `BASE_URL=`뒤에 본인의 baseUrl 주소를 넣으시면 됩니다.

``` dart
void main() {
  AuthRepository.initialize(appKey: 'javascript key');
}

or

void main() {
  AuthRepository.initialize(appKey: 'javascript key', baseUrl: 'http://localhost');
}
```

### Android

AndroidManifest.xml 에 INTERNET 권한 및 usesCleartextTraffic="true" 설정

``` xml
<manifest>
    <!-- webview_flutter 에서 인터넷 접속을 위한 권한을 선언합니다 -->
    <uses-permission android:name="android.permission.INTERNET" />

    <application
    android:usesCleartextTraffic="true">
        ...
    </application>
</manifest>
```

### iOS

Info.plist 에 NSAppTransportSecurity 권한 및 io.flutter.embedded_views_preview 설정

``` xml
<dict>
    <key>NSAppTransportSecurity</key>
      <dict>
        <key>NSAllowsArbitraryLoads</key>
        <true/>
        <key>NSAllowsArbitraryLoadsInWebContent</key>
        <true/>
      </dict>
    <key>io.flutter.embedded_views_preview</key>
    <true/>
</dict>
```

---

## 예제

[Kakao maps api](https://apis.map.kakao.com/web/sample/) 사이트에 있는 예제를 기준으로 샘플을 만들었습니다.

* 기본 지도 생성

    ``` dart
    Scaffold(
      body: KakaoMap(),
    );
    ```

* 맵 생성 callback

    ``` dart
    Scaffold(
      body: KakaoMap(
        onMapCreated: ((controller) {
          mapController = controller;
        }),

      ),
    );
    ```

* 마커 생성 - 지도가 생성되면 마커 추가되는 예제

    ``` dart
    Set<Marker> markers = {}; // 마커 변수
  
    Scaffold(
      body: KakaoMap(
        onMapCreated: ((controller) async {
          mapController = controller;

          markers.add(Marker(
            markerId: UniqueKey().toString(),
            latLng: await mapController.getCenter(),
          ));

          setState(() { });
        }),
        markers: markers.toList(),
        center: LatLng(37.3608681, 126.9306506),
      ),
    );
    ```

* 마커 클러스터 생성 - 지도가 생성되면 마커 추가되는 예제 (마커와 클러스터 함께 사용하지 마세요. 클러스터 안에 마커를 넣어서 사용하세요.)

    ``` dart
    Clusterer? clusterer;
  
    Scaffold(
      body: KakaoMap(
        onMapCreated: ((controller) async {
          mapController = controller;

          Set<Marker> markers = {};
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.27943075229118, 127.01763998406159)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.55915668706214, 126.92536526611102)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.13854258261161, 129.1014781294671)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.55518388656961, 126.92926237742505)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.20618517638034, 129.07944301057026)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.561110808242056, 126.9831268386891)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.86187129655063, 127.7410250820423)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.47160156778542, 126.62818064142286)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.10233410927457, 129.02611815856181)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.10215562270429, 129.02579793018205)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.475423012251106, 128.76666923366042)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.93282824693927, 126.95307628834287)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.33884892276137, 127.393666019664)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.520412849636, 126.9742764161581)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.155139675209675, 129.06154773758374)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.816041994696576, 127.11046706211324)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(38.20441110638504, 128.59038671285234)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.586112739308916, 127.02949148517999)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.50380641844987, 127.02130716617751)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.55155704387368, 126.92161115892036)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.55413060051369, 126.92207472929526)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.362321615174835, 127.35000483225389)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.55227862908755, 126.92280546294998)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.490413948014606, 127.02079678472444)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.172358507549596, 126.90545394866643)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.15474103200252, 129.11827889154455)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.516081250973485, 127.02369057166361)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.80711722863776, 127.14020346037576)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.28957415752673, 127.00103752005424)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.83953896766896, 128.7566880321854)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.51027412948879, 127.08227718124704)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.493581783270294, 126.72541955660554)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.135291862962795, 129.10060911448775)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.174574933144065, 126.91389980787773)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.795887691878654, 127.10660416587146)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.59288687521181, 126.96560524627377)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.45076411130452, 127.14593003749792)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.86008337557079, 127.1263912488061)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.23773491330953, 129.08371037429578)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.524297321304886, 127.05018281937049)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.33386658021849, 127.4461721466889)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.72963747546802, 128.27079056365005)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.02726828142973, 129.37257233594056)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.0708030360945, 129.0593185494088)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.86835862950247, 128.59755089175871)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(33.51133264696746, 126.51852347452322)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.61284289586752, 127.03120547238589)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.851696038722466, 128.59092937125666)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.59084695083232, 127.01872773588882)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.52114874288784, 129.33573629945764)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.362326407439845, 127.33577420148076)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.28941189110747, 127.00446132665141)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.32049801117398, 129.1810343576788)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.53338631541601, 127.00615481678061)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.413461468258156, 126.67735680840826)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.920390371093205, 128.54411720249956)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.65489374054824, 127.48374816871991)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.49491987110441, 127.01493134206048)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.64985695608336, 127.14496345268074)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.55686770317417, 127.16927880543041)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.37014007589146, 127.10614330185591)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.5350236507627, 126.96157681184789)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.40549630594667, 126.8980581820004)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(34.867950544005744, 128.69069690081176)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.16317059543225, 128.98452978748048)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.607484825953186, 127.48520451195111)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.651724785213986, 126.58306748337554)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.86059690063427, 128.59193087665244)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.25685847585025, 128.59912605060455)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(33.509258155694496, 126.5109451464813)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.64366155701157, 126.63255039247507)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.82667262227336, 127.1030670574823)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.82003554991111, 127.14810974062483)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.097485195649455, 128.99486181862338)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.32204249590605, 127.95591893585816)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.50535127272031, 127.1047465440526)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.99081407156533, 127.09338324956647)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.323486640444834, 127.12285239871076)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.78973089440451, 127.13644319545601)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.641373953578196, 129.35463220719618)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.47423127310911, 126.97625029161996)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.84357192991226, 128.61143720719716)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.180974984085736, 128.20294526341132)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.57895718642583, 126.9316897337244)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(33.49077253755052, 126.49314817000993)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.42175925330255, 128.67409133225766)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.46405540570109, 126.7153544119173)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.594758776232126, 127.10099917489818)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.7239966558994, 127.0478671731854)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.86680171505329, 128.5923738376741)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.560573727266785, 126.81239107485251)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.78692224857484, 126.98966010341789)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.10368644802913, 129.0206862606022)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.063839948992644, 127.06856523030079)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.34344643728643, 127.94382181350932)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.512521267219064, 127.40054805648133)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.15286653837983, 126.90419903971498)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.173238445546296, 129.176082844468)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.082394201323524, 129.40330471725923)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.51043665598106, 127.03974070036524)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.627816673285054, 127.44969866021904)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.59194624756919, 127.01817545576053)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.387147045560866, 127.1253365438929)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.89948383848115, 128.60809550730653)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.555316235235324, 127.14038447894715)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.09622092762977, 128.43314679004078)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.582855922985544, 126.91907857008522)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.516000983841586, 128.72798872032757)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.48429363675198, 127.0379630203579)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.54502575965604, 126.95429338245707)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.236247173046394, 128.8677618015292)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.40157536691968, 127.11717457214067)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.95191038001258, 127.91064040877527)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.491526492971346, 126.85463749525812)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(36.124356479753196, 128.09517052346138)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.15715169307048, 128.15853461363773)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.5808156608605, 126.95109705510639)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.46931787249714, 126.89904775044873)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.52195614910054, 129.3209904841746)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.58625703195563, 126.9496035206742)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.28463639199199, 126.85984474757359)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.534169458631226, 129.31169021536095)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.553341234194285, 127.15481222237025)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(37.62293367990081, 126.83445005122417)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.5272027005698, 127.72953798950101)));
          markers.add(Marker(
              markerId: '${markers.length + 1}',
              latLng: LatLng(35.180032285898854, 128.06954509175367)));
  
          clusterer = Clusterer(
            markers: markers.toList(),
            minLevel: 6,
            gridSize: 45,
            calculator: [30, 60],
            texts: ['적음', '보통', '많음'],
            styles: [
              ClustererStyle(
                width: 50,
                height: 50,
                background: Colors.blue.withOpacity(0.8),
                borderRadius: 25,
                color: Colors.white,
                textAlign: 'center',
                lineHeight: 60,
              ),
              ClustererStyle(
                width: 50,
                height: 50,
                background: Colors.red.withOpacity(0.8),
                borderRadius: 25,
                color: Colors.yellow,
                textAlign: 'center',
                lineHeight: 60,
              ),
              ClustererStyle(
                width: 50,
                height: 50,
                background: Colors.purple.withOpacity(0.8),
                borderRadius: 25,
                color: Colors.white,
                textAlign: 'center',
                lineHeight: 60,
              ),
            ],
          );

          setState(() { });
        }),
        clusterer: clusterer,
        center: LatLng(37.3608681, 126.9306506),
      ),
    );
    ```

* Circle, Polyline, Polygon, Rectangle 예제

    ``` dart
    Set<Circle> circles = {};
    Set<Polyline> polylines = {};
    Set<Polygon> polygons = {};
    Set<Rectangle> rectangles = {};
  
    Scaffold(
      appBar: AppBar(
        title: Text(widget.title ?? selectedTitle),
      ),
      body: KakaoMap(
        onMapCreated: ((controller) async {
          mapController = controller;

          circles.add(
            Circle(
              circleId: circles.length.toString(),
              center: LatLng(33.450701, 126.570667),
              strokeWidth: 5,
              strokeColor: Colors.red,
              strokeOpacity: 0.5,
              strokeStyle: StrokeStyle.longDashDotDot,
              fillColor: Colors.black,
              fillOpacity: 0.7,
              radius: 50,
            ),
          );

          polylines.add(
            Polyline(
              polylineId: 'polyline_${polylines.length}',
              points: [
                LatLng(33.452344169439975, 126.56878163224233),
                LatLng(33.452739313807456, 126.5709308145358),
                LatLng(33.45178067090639, 126.5726886938753)
              ],
              strokeColor: Colors.purple,
            ),
          );

          polygons.add(
            Polygon(
              polygonId: 'polygon_${polygons.length}',
              points: [
                LatLng(33.45133510810506, 126.57159381623066),
                LatLng(33.44955812811862, 126.5713551811832),
                LatLng(33.449986291544086, 126.57263296172184),
                LatLng(33.450682513554554, 126.57321034054742),
                LatLng(33.451346760004206, 126.57235740081413)
              ],
              strokeWidth: 4,
              strokeColor: Colors.blue,
              strokeOpacity: 1,
              strokeStyle: StrokeStyle.shortDashDot,
              fillColor: Colors.black,
              fillOpacity: 0.3,
            ),
          );
  
          rectangles.add(
            Rectangle(
              rectangleId: 'rectangle_${rectangles.length}',
              rectangleBounds: LatLngBounds(
                LatLng(33.42133510810506, 126.53159381623066),
                LatLng(33.44955812811862, 126.5713551811832),
              ),
              strokeWidth: 6,
              strokeColor: Colors.blue,
              strokeOpacity: 1,
              strokeStyle: StrokeStyle.dot,
              fillColor: Colors.black,
              fillOpacity: 0.7,
            ),
          );

          setState(() {});
        }),
        circles: circles.toList(),
        polylines: polylines.toList(),
        polygons: polygons.toList(),
        rectangles: rectangles.toList(),
        center: LatLng(33.450701, 126.570667),
      ),
    );
    ```

더 많은 카카오지도 샘플소스는 **[여기](https://github.com/johyunchol/kakao_map_plugin/tree/main/example)** 에서 확인하실 수 있습니다.

---

## 실행화면

![example](https://github.com/johyunchol/kakao_map_plugin/blob/main/assets/videos/example.gif?raw=true)
