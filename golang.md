***
title: Golang
date: 2020-04-03 20:36:49+01:00
author: Gaxpert
***

#Golang

## General use

go run FILE --> Only runs
go build FILE --> Compiles
gofmt FILE -w out_file--> Formats code
go tools
godoc --http=:8080 --> opens go docs in browser
goimports --> Removes unnecessary imports

### Build for other archs
env GOOS=target-OS GOARCH=target-architecture go build package-import-path

target-OS = windows / linux / android ....
target-arch = arm / 386 / amd64

### Loops
through map
for _, M := range MAP {
  fmt.print( M   )
}
_  --> index element
M --> map element


## Args
os.Args --> arguments package

slices --> [m:n] = m though n-1

### Declare var
s := "" //short var declaration, only inside function
var s string //usual
var s = ""
var s string = 
(Use first two)


## Printf
%d de cimal integer
%x, %o, %b integer in hexade cimal, octal, binar y
%f, %g, %e floating-p oint number: 3.141593 3.1
%t boole an: true or false
%c rune (Unico de co de point)
%s st ring
%q quot ed str ing "abc" or rune 'c'
%v any value in a natural for mat%T type of any value
%% literal percentt sign 


### Builtin types
  Constants: 
true false iota nil
  Types: 
int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uint ptr float32 float64 complex128 complex64
bool byte rune string error
  Functions: 
make len cap new append copy close delete complex real imag
panic recover


### Packages
flag for arguments:
ex:
  var n = flag.Bool("n", false, "description")
  (in main)
  flag.Parse()
  (call variable pointer)
  *n
usage:
  script.go -n
  
### Composite Types
arrays --> aggregate type, homogeneous (all elements have same type), fixed size
structs --> aggregate type, heterogenous, fixed size
slices --> dinamic data struct  
maps    --> dinamic data struct


### Notes
When a function is called, a copy of each arg value is assigned to the parameter = inefficient to pass big values (arrays)



### Libraries
Color -> Color cmd
cli -> command line apps
authboss -> web auth system
docker
cobra -> cmd
Gen -> generate code
Gorm -> Dbs
Goose -> Dbs
Ginkgo -> Testing framework

### Sizes
Single file -> 1mb
With fmt -> 2mb

### change struct var
var p = &person{}
if key == "age" {
    p.age = value
}

### Hugo
Static site gen

#### Start project 
hugo new site SITENAME
#### live server
hugo server
#### so it drafts draft files
hugo server -D

config.toml --> theme = "THEME"

#### Cool themes
    
Single page

[GitHub - Somrat37/somrat: hugo portfolio theme](https://github.com/Somrat37/somrat)
[Meghna](https://themes.gohugo.io/theme/meghna-hugo/)
[Hugo Themes](https://themes.gohugo.io/theme/air/)

More Pages
[Hello Friend NG — Hello Friend NG Theme](https://themes.gohugo.io//theme/hugo-theme-hello-friend-ng/)


### Web Assembly

    Compilation
    
Can only compile main packages!

[WebAssembly · golang/go Wiki · GitHub](https://github.com/golang/go/wiki/WebAssembly)
#### compile
GOOS=js GOARCH=wasm go build -o main.wasm

#### get js support file
cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .

create index.html (check git)
  
#### run server
goexec 'http.ListenAndServe(`:8080`, http.FileServer(http.Dir(`.`)))'
  



## Libs

### Cobra
Cobra -> command line gen
[GitHub - spf13/cobra: A Commander for modern Go CLI interactions](https://github.com/spf13/cobra#overview)

#### Install cobra
`go get github.com/spf13/cobra/cobra`

#### cobra generator
cd into app dir
`cobra init --pkg-name newApp`

#### create aditional commands
`cobra add EXAMPLE`

#### create subcommands
`cobra add SUBCOMMAND -p 'commandCmd'`

config at: ~/.cobra.yalm
Sample config:

> author: Gaxpert <Gaxpert@protonmail.com>
> license: none

Persistent flag --> available to every command
`rootCmd.PersistentFlags().BoolVarP(&Verbose, "verbose", "v", false, "verbose output")`

Local flag --> only apply to that command
`localCmd.Flags().StringVarP(&Source, "source", "s", "", "Source directory to read from")`

Required flags
`rootCmd.MarkFlagRequired("region")`


### Color
`go get github.com/fatih/color`

`import "github.com/fatih/color"`

Standard colors
```go
// Print with default helper functions
color.Cyan("Prints text in cyan.")

// A newline will be appended automatically
color.Blue("Prints %s in blue.", "text")

// These are using the default foround colors
color.Red("We have red")
color.Magenta("And many others ..")
```

### Log
log.Fatal() logs error and exits (better than os.exit()

log.Printf() --> good for logging results (instead of fmt)

log.Panicf --> similar to fatal but prints more debug info

### Regex
import regexp

```go
parse, err := regexp.Compile("[mM]ihalis")
parse.MatchString("string")
parse.ReplaceAllString(....)
```

### go-callvis
Go code visualizer

`go get -u github.com/TrueFurby/go-callvis`
`pacman -S graphviz`

#### run
`go-callvis main.go`


### WebScrapping

goquery lib
`go get github.com/PuerkitoBio/goquery`


#### colly
`go get -u github.com/gocolly/colly/...`

```go
import "github.com/gocolly/colly"

//main entity is a Collector object

//Initialize collector
c1 := colly.NewCollector()

//Collector atributes full list
[colly - GoDoc](https://godoc.org/github.com/gocolly/colly#Collector)
```

## Web Programming
use form-urlencoded for simple things
use multipart/form-data for big things like file uploads


### Cookies
#### Set cookies
```go
c:= http.Cookie{
Name: "asd",
.....
}
http.SetCookie(w, &c)
```
#### Get cookies
```go
r.Cookie("name")
r.Cookies() #all cookies
```

#### Remove cookie
Set cookie with same name and 
MaxAge: -1
Expires: time.Unix(1,0)

### Template
Go web template engine requires:
1) Parse the text-formatted template
2) Execute the parsed template passing a ResponseWriter and some data
Handling error:
`t := template.Must(template.ParseFiles("file"))`
panics if error != nil

  In template file
{{ . }}  --> Data
#### Conditionals
```
{{ if . }}
bla
{{ else }}
bleh
{{ end }}
```
#### Iterators
{{ range array }}
dot is set to the element {{ . }}
{{ end }}
  Nested template
{{ template "t2.html" . }} 
(We can pass args on the template)
  Functions
#### Pass fun as funcMap
```go
funcMap := template.FunMap{ "func": func }
t := template.New("tmp1.html").Funcs(func)
```
Parse
Execute

#### Go sanitaze input
In case we dont want that (run code)
`t.Execute(2, template.HTML(r.FormValue("comment")))`
Typecast comment value in html template

Default template
{{ block "content" . }}
bla {{ end }}
If no content template is specified, it will use this block 	

