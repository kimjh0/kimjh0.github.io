---
title: "Visual Studio custom path"
tags:
  - windows
---

## 증상

- 로컬에 설치된 vs 에서 라이브러리 및 헤더 경로를 못 찾음
- win32 빌드시 항상 발생
- custom path 지정 하기 위해 기록

## 방법

- vs 프로젝트의 속성을 확인 (vcxproj 파일)

  ```
  ...
  <ImportGroup Label="PropertySheets">
    <Import Project="$(UserRootDir)\Microsoft.Cpp.$(Platform).user.props" Condition="exists('$(UserRootDir)\Microsoft.Cpp.$(Platform).user.props')" Label="LocalAppDataPlatform" />
  </ImportGroup>
  ...
  ```

- 해당 위치로 이동하여 파일 수정
  - windows 10 에서 C:\Users\User\AppData\Local\Microsoft\MSBuild\v4.0 경로로 이동
  - Microsoft.Cpp.Win32.user.props 파일 수정
  
  ```
  <?xml version="1.0" encoding="utf-8"?>
  <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
      <IncludePath>$(VC_IncludePath);$(VCInstallDir)include;$(VCInstallDir)atlmfc\include;$(WindowsSdkDir)include;$(FrameworkSDKDir)\include</IncludePath>
      <LibraryPath>$(VC_LibraryPath_x86);$(VCInstallDir)lib;$(VCInstallDir)atlmfc\lib;$(WindowsSdkDir)lib;$(FrameworkSDKDir)\lib</LibraryPath>
    </PropertyGroup>
  </Project>
  ```
 
- 각 매크로는 vs 의 프로젝트 속성에서 경로를 지정할때 매크로 버튼을 통해 값을 확인 할 수 있음
