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
to read more click  [https://www.rplumber.io/][HERE] 


## step 5 
now everything should be fine, you can dockerise your code and ship it anywhere you want 

```sh
$ touch Dockerfile
```
append following lines

```sh
FROM trestletech/plumber

#to install packages uncoment following line
#RUN R -e "install.packages('whatever it is used in the code')"


#for apt-get uncomment & modify following line 
#RUN apt-get update -qq && apt-get install whatever is needed 

#expose a port 
EXPOSE 8000

#make a folder and copy your files 
RUN mkdir /app
COPY funcs.R /app
COPY plumber.R /app
WORKDIR /app

#set entrypoint
ENTRYPOINT ["R", "-e", "pr <- plumber::plumb('plumber.R'); pr$run(host='0.0.0.0', port=8000,swagger=TRUE)"]
```

build your docker and forward port 8000 from your container to any port you want on host 



