# dockerize_R_expose_as_api
imagine you have an peice of code developed by your data scientist/ Mathematician/Statistician or whoever else that is happy to work with R, now your devs are required to plug in that peice of code to your project. i do not need to imagine it cuz it happened for me and this is how i solved it 

## step 0 
lets start from creating a folder for this project 
  ```sh
$ mkdir exposemyRcodeasAPI
$ cd  exposemyRcodeasAPI/
```
create a text file name it plumber.r
  ```sh
$ touch plumber.r 
```
then open plumber file with your text editor
```sh
$ nano plumber.r 
```

 ## step 1 
 add following to head of your file 
 ```sh
library(fitdistrplus)
library(scales)
library(actuar)
library(plumber)
```
## step 2 
source the R code (lets call it func.R)
 ```sh
source("funcs.R")
```

## step 3 
at run time your plumber file will generate swagger docs by adding following line you will set your apititle (append following text to plumber.R file after sourcing the R code)

```sh
#* @apiTitle yourapinamehere
```

## step 4
wrapp whichever function that you want to expose in a function with following declaration 

```
#* Echo back the input
#* @param msg The message to echo
#* @get /echo
function(msg = "") {
    somefuncInFuncdotRFile(msg)
}
```

if you want to have post method use (#* @post instead of get)
to read more about how to declare api in plumber 
