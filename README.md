# New Feature

- fix [issue #56567](https://github.com/golang/go/issues/56567) —— x/mobile:multiple libraries in android.
- modify generated c-shard library 's name which determined by your first package name. Fox example,
  if your first package name (from the array result of package scanning ) is "package test", so that
  the c-shard library name would be "libtest.so"

## How to import multiple android archive into android studio

1. initialize your go module

   i. go mod init <ModuleName>

   ii. replace gomobile remote repository using command : `go mod edit -replace=golang.org/x/mobile@latest=github.com/MrKrisYu/mobile@latest`,or manually edit go.mod as following

   ```go
   module bonjour
   
   go 1.21.1
   
   replace golang.org/x/mobile v0.0.0 => github.com/MrKrisYu/mobile v0.0.0-20230925073923-bf58556eb872
   
   require (
   golang.org/x/mobile v0.0.0 // indirect
   golang.org/x/mod v0.12.0 // indirect
   golang.org/x/sys v0.12.0 // indirect
   golang.org/x/tools v0.13.0 // indirect
   ) 
   ```

   iii. get gomobile/bind dependency

   ```go
   go env -w GOPROXY=https://goproxy.cn
   go get golang.org/x/mobile/bind
   ```



2. install gomobile

   ​	i. git clone `https://github.com/MrKrisYu/mobile.git`, then cd mobile/cmd/gomobile directory,
   finally install gomobile.exe or gomobile(based on your os) using command: `go install .`

   ​	ii. initialize gomobile(On condition that you have already prepare NDK, Android SDK, JDK, ) using command: `gomobile init`

   ​	iii. due to `gombile init` will automatically install gobind from golang.org/x/mobile, you need cd previous cloned project 's  mobile/cmd/gobind directory and then open terminal and run command `go install .` to replace gobind exectuable file.

3. compile AAR files

   if you successful get gomobile executable file, now you are able to build  aar using command: `gomobile bind -target=android <YourPackage> -v`



4. Import multi aar into Android Studio Project(The following will be referred to AS)

   ​	i. copy your aar files and `gomobile-java.jar`(at libs directory) into your libs directory in AS project

   ​	ii. add some configuration into your application 's settings.gradle as following

```gradle
//=============setting.gradle=================
dependencyResolutionManagement {
    repositories {
        flatDir {
            dirs 'libs'
        }
    }
}
//=============build.gradle(Your Application)======   
dependencies {
    implementation(name: 'AAR-1-Module-From-Gomobile', ext: 'aar')
    ...
    implementation(name: 'AAR-N-Module-From-Gomobile', ext: 'aar')
    implementation(name: 'gomobile-java', ext: 'jar')
}   
```

​	iii. sync your gradle setting, now you are able to use your libs.



# Go support for Mobile devices

[![Go Reference](https://pkg.go.dev/badge/golang.org/x/mobile.svg)](https://pkg.go.dev/golang.org/x/mobile)

The Go mobile repository holds packages and build tools for using Go on mobile platforms.

Package documentation as a starting point:

- [Building all-Go apps](https://golang.org/x/mobile/app)
- [Building libraries for SDK apps](https://golang.org/x/mobile/cmd/gobind)

![Caution image](doc/caution.png)

The Go Mobile project is experimental. Use this at your own risk.
While we are working hard to improve it, neither Google nor the Go
team can provide end-user support.

This is early work and installing the build system requires Go 1.5.
Follow the instructions on
[golang.org/wiki/Mobile](https://golang.org/wiki/Mobile)
to install the gomobile command, build the
[basic](https://golang.org/x/mobile/example/basic)
and the [bind](https://golang.org/x/mobile/example/bind) example apps.

--

Contributions to Go are appreciated. See https://golang.org/doc/contribute.html.

* Bugs can be filed at the [Go issue tracker](https://golang.org/issue/new?title=x/mobile:+).
* Feature requests should preliminary be discussed on
[golang-nuts](https://groups.google.com/forum/#!forum/golang-nuts)
mailing list.
