# android-clean-architecturea 레포지토리 분석하고 부족한점 채우기!

https://github.com/ydhnwb/android-clean-architecture

안드고님이 추천해주신 coroutine, hilt, flow를 사용한 프로젝트이다.

위 기능들을 더 딥하게 공부하기에 좋은 앱인 것 같아 분석해보기로 했다. -> 서버에러로 실행은 불가능하다.

## 프로젝트 폴더 구조
- data
  - login
  - product
  - register
  
- domain
  - login
  - product
  - register

- presentation
  - login
  - main
  - register

data 패키지(레이어)는 remote, repository 패키지가 존재하고, Hilt 주입을 위한 Hilt Module Class가 존재한다.

domain 패키지(레이어)는 entity, usecase 패키지가 존재하고, Datasource에 접근하는 Repository가 존재한다.

presentation 패키지(레이어)는 login, main, register 패키지가 존재하고, Activity, Fragment, ViewModel 등 UI 컴포넌트와 Android Framework 관련된 코드를 포함한다.







## 프로젝트를 분석하면서 나에게 적용하면 좋은 것과 길러야할 개발 습관 

### **1. Clean Architecture 구조 파악.**

클린 아키텍처의 각 계층간 동작 이해하고, 관심사 분리 확실하게 하기.

<img width="656" alt="image" src="https://user-images.githubusercontent.com/70135188/227481409-8effe446-132d-45a9-b325-c7c50c953c05.png">


### **2. sealed class 사용해서 response 상태별로 관리하기.**
```kotlin
sealed class RegisterActivityState {
    object Init : RegisterActivityState()
    data class IsLoading(val isLoading: Boolean) : RegisterActivityState()
    data class ShowToast(val message: String) : RegisterActivityState()
    data class SuccessRegister(val registerEntity: RegisterEntity) : RegisterActivityState()
    data class ErrorRegister(val rawResponse: WrappedResponse<RegisterResponse>) : RegisterActivityState()
}
```


### **3. Utils 클래스 만들어서 자주 사용되는 기능 확장함수로 관리하기.**
```kotlin
fun Context.showToast(message: String){
    Toast.makeText(this, message, Toast.LENGTH_LONG).show()
}

fun Context.showGenericAlertDialog(message: String){
    AlertDialog.Builder(this).apply {
        setMessage(message)
        setPositiveButton(getString(R.string.button_text_ok)){ dialog, _ ->
             dialog.dismiss()
        }
    }.show()
}
```

### **4. StateFlow, SharedFlow 상황에 맞게 사용하기**

### **5. OkHttp 세부사항 적용하기**

ex) connectTimeout, readTimeout 등

### **6. 패키지 구조를 모듈화로 변경 적용**



## 분석하면서 공부한 것들
1. [Android Clean Architecture](https://spicy-spandex-e82.notion.site/Android-Clean-Architecture-8140b20e87ff42d6a694ddde445d0db5)
2. [클린 아키텍처 모듈화](https://spicy-spandex-e82.notion.site/cef228509d7d45cfa5f79e316530c6f1)
3. [Retrofit 사용시 알아두면 좋은 것 - 내부 동작](https://www.notion.so/dev-jiwon/Retrofit-d0823f1d96c14c3d96692fb4fe69e4d1)



## 분석 후 만든 앱✌️
