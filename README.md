# flutter_getx_dependency_management

GetX를 사용하여 종속성을 관리하기

## Getting Started

# Get.put
GetX에서 종속성을 관리하기 위해 Get.put을 사용한다.

Get.out(ExController());
Get.put으로 추가한 컨트롤러를 변수에 할당하여 사용할 수 있다.
```
final controller = Get.put(ExController());
...
print(controller.count.value);
...
controller.function();
```
# Get.lazyPut
Get.put은 해당 코드가 실행되는 시점에 컨트롤러가 메모리에 생성되게 된다.
하지만 Get.lazyPut을 사용하면 컨트롤러를 사용하는 시점에 컨트롤러가 메모리에 생성되게 된다.

# Get.putAsync
추가하려는 컨트롤러가 Future를 반환하는 경우, 다음과 같이 Get.putAsync을 사용하여 비동기 컨트롤러를 처리할 수 있다.

# Get.find
Get.put, Get.lazyPut, Get.putAsync을 사용하여 컨트롤러를 추가하면, Get.find를 사용하여 추가한 컨트롤러를
찾아서 사용할 수 있다,

# 화면 이동시 종속성 주입
Get.to로 화면을 이동할 때 binding을 사용하여 컨트롤러를 생성, 주입할 수 있다.
```
Get.to(Second(), binding:BindingsBuilder((){
    Get.put(CountController());
}));
```
또는 Get.toNamed에도 사용할 수 있다.
```
Get.toNamed('/second',binding.BindingsBuilder((){
    Get.put(CountController());
}));
```

# 라우트에서 종속성 주입
GetPage에서 binding을 사용하여 컨트롤러를 생성, 주입할 수 있다.
```
getPages: [
    GetPage(name:"/", page: () => Home()),
    GetPage(name:"/second", page: () => Second(), binding: BindingsBuilder(() {
    Get.lazyPut<CountController>(() => CountController());
    })),
]
```

# binding 분리
Get.to/Get.toNamed 또는 GetPage에서 binding을 직접 사용할 수 있지만, 다음과 같이 binding을 분리할 수 있다.
```
class SecondBinding implements Bindings{
 @override
 void dependencies(){
  Get.lazyPut<CountController>(() => CountController());
 }
}
```
이렇게 생성한 binding을 Get.to/Get.toNamed에서 사용할 수 있다.
```
Get.to(Second(), binding:binding:SecondBinding());
```
또는 다음과 같이 GetPage에서도 사용할 수 있다.
```
getPages:[
 GetPage(name:"/", page: () => Home()),
 GetPage(name:"/second", page: () => Second(),binding:SecondBinding()),
]
```

# permanent와 GetxService
GetX에서 컨트롤러는 사용이 종료되면 자동으로 메모리에서 제거된다. 
하지만 미리 데이터를 로드하거나 자주 사용되는 컨트롤러는 미리 메모리에 로드하고,
로드된 컨트롤러를 제거하지 않고 계속 사용하는 경우가 있다. 이럴 때 permanent 옵션을 사용할 수 있다.
또는 GetxController를 사용하는 것이 아니라 GetxService를 사용하여 제거되지 않는 컨트롤러를 생성할 수 있다.

# Get.reset
이미 메모리에 추가된 컨트롤러를 초기화해야 할 경우 Get.reset을 사용한다. 주로 테스트 코드를 작성할 때 사용한다.

# Get.remove
거의 사용되지 않지만, 특정한 상황에서 메모리에 등록된 컨트롤러를 제거할 필요가 있을 때, Get.remove를 사용할 수 있다.
GetX는 컨트롤러의 사용이 종료되면, 자동으로 메모리에서 제거되므로 보통은 사용할 일이 없다.

# 출처
* https://dev-yakuza.posstree.com/ko/flutter/getx/dependency/