### Swift Package Plugin이란?

- Swift Package는 Xcode 11에서 소개되어, 라이브러리를 배포하는데에 훌륭한 방법을 제공하였다.
- Swift Package Plugin은 Xcode 14에서 소개되었고 스위프트 패키지의 기능을 확장하여 빌드를 하는 동안에 또는 빌드를 하기 전에 필요한 소스코드를 생성하는 등의 기능을 제공할 수 있게 만들어준다
- Swift Package Plugin에는 총 2종류의 plugin이 있다.
    - **Build plugin**
    - Command plugin

### Build Plugin

- Build plugin은 빌드 중이거나 빌드가 되기 전에 어떤 input 파일을 실행시켜서 output 파일들을 생성하는 플러그인이다
- 즉 어떠한 소스파일을 생성하거나 리소스를 생성해야할 때 유용하게 사용할 수 있음
- Command plugin은 직접 실행시켜줘야하는 반면, Build plugin은 자동으로 실행된다는 점에서 좀 더 유용하다(Command plugin은 실행을 까먹기 쉬움)
- Build 플러그인은 2가지 종류로 나뉘어짐
    - pre-build plugin
    - build plugin
    

## Build plugin 적용하기

- 우선 plugin을 선언해주기 위해 우리는 Package.swift 파일이 필요하다. SPM에서만 사용할 수 있기 때문
- Swift Package plugin을 사용해주기 위해서는 반드시 swift-tools-version을 5.7 이상으로 설정해줘야 한다.

```swift
import PackageDescription

let package = Package(
  name: "SwiftLintPlugin",
  platforms: [ .iOS(.v16), .macOS(.v10_13)],
  products: [
    // Products define the executables and libraries a package produces, and make them visible to other packages.
    .plugin(
      name: "SwiftLintPlugin",
      targets: ["SwiftLintPlugin"]
    )
  ],
  targets: [ 
    .plugin(
      name: "SwiftLintPlugin",
      capability: .buildTool()
    )
  ]
)
```

- 예시 코드
- 만약 우리가 사용하는 플러그인이 다른 타겟이나 패키지에 의존하고 있다면 capability 아래에다가 dependencies 프로퍼티를 추가해주면 된다.

### Plugin 폴더 구조

- 만약 올바른 폴더 구조를 갖추지 못했다면, 에러가 발생하게 됨.
- SPM은 플러그인을 위한 적절한 폴더구조를 갖춰야만 올바르게 동작함
    - Package.swift 폴더 아래에 Plugins 폴더를 만들어주자
    - Plugins폴더 안에다가 내가 만들고자 하는 플러그인 이름의 폴더를 만들자(ex. SwiftLintPlugin)
    - 플러그인 이름 폴더 안에 우리가 실행하려고 하는 Plugin.swift 파일을 넣어두자. 이 때 이 파일의 이름은 중요하지 않음.
        
        ![스크린샷 2023-01-26 오후 8 38 03](https://user-images.githubusercontent.com/87598209/222444726-9cc9f71f-9c63-45a2-8296-60e07c813624.png)


### Plugin 실행하기

- 플러그인을 다 만들어줬다면 이제 실행만하면 된다
- 실행 전 몇 가지 단계를 준수해줘야 한다
    - PackagePlugin 프레임워크를 import 해줘야 한다. 해당 프레임워크에 플러그인 관련 새로운 API들이 포함되어 있기 때문
    - 우리들이 만든 플러그인 코드들을 포함하는 구조체를 만들어준다
    - 해당 구조체에 @main 어노테이션을 붙여준다. 이 표시는 플러그인의 엔트리 포인트를 의미
    - 해당 구조체가 BuildToolPlugin 프로토콜을 준수해야한다
    - `createBuildCommands(context:,target:) async throws -> [Command]` 메서드를 정의해준다
    
    ```swift
    import PackagePlugin
    import Foundation
    
    @main
    struct MySwiftLintPlugin: BuildToolPlugin {
      func createBuildCommands(context: PluginContext, target: Target) async throws -> [Command] {
        let dir = context.pluginWorkDirectory.appending("swiftlint")
        try? FileManager.default.removeItem(atPath: dir.string)
        try FileManager.default.createDirectory(atPath: dir.string, withIntermediateDirectories: false)
    
        let current = FileManager.default.currentDirectoryPath
    
        return [
          .prebuildCommand(
            displayName: "MySwiftLint",
            executable: .init("/path/to/bin/swiftlint"),
            arguments: [current],
            outputFilesDirectory: dir
          )
        ]
      }
    ```
    
- 예시 코드
- 코드를 봤을 때, 전체적인 실행과정은 기존에 설치되어있던 디렉토리가 있다면 제거해주고 새로운 디렉토리를 생성해준다.
- 현재 경로를 저장해서 SwiftLint의 lint 파일로 전달한다
- 이후, .prebuildCommand를 리턴해준다
    - prebuildCommand의 일부 파라미터를 살펴보면,
        - displayname은 Xcode build step상에서 표시될 이름이다
        - executable은 실행시키고 싶은 파일을 의미

### 다른 패키지에서 만들어둔 plugin 사용하기

- 간단하게 dependencies에 우리가 만든 플러그인을 추가만 하면 된다 !

```swift
// swift-tools-version: 5.7
// The swift-tools-version declares the minimum version of Swift required to build this package.

import PackageDescription

let package = Package(
    name: "MyConsoleApp",
    dependencies: [
        // Dependencies declare other packages that this package depends on.
        // .package(url: /* package url */, from: "1.0.0"),
+      .package(path: "../SwiftLintPlugin")
    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages this package depends on.
        .executableTarget(
            name: "MyConsoleApp",
            dependencies: [],
+            plugins: [.plugin(name: "SwiftLintPlugin", package: "SwiftLintPlugin")],
        ),
        .testTarget(
            name: "MyConsoleAppTests",
            dependencies: ["MyConsoleApp"]),
    ]
)
```

- 이제 해당 패키지를 빌드하면, 아래처럼 SwiftLint가 잘 적용된 것을 확인할 수 있다.

![스크린샷 2023-01-26 오후 9 15 09](https://user-images.githubusercontent.com/87598209/222444778-8b03c5b4-e66d-45cf-9b07-265aced22b17.png)

- 플러그인이 제대로 실행됐는지 확인하고 싶으면 리포트 네비게이터를 확인하자

![스크린샷 2023-01-26 오후 9 16 42](https://user-images.githubusercontent.com/87598209/222444820-18aa0f7f-fb2f-42d2-bc57-4e2609ea9bcc.png)
