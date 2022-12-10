# Automatic Number Plate Recognition  (ANPR)

 image processing technology  which is used to identify vehicles by their  number plates using Optical Character  Recognition.


## Table of Contents

1. [API Reference](#API_Reference)
2. [Environment Variables](#Environment_Variables)
3. [Programmers](#Programmers)
4. 
## API Reference

This system has three main files `template_creation.m`,`Letter_detection.m` and 
`Plate_detection.m`

#### **Template Creation**

Template Creation file is used to call the saved images of alphanumerics and then save them as a new template in MATLAB memory
- Use `imread()` function to call and read the images from the folder or from any location of the PC into the MATLAB.
- Assign variables with the same names of the images to save them.

```
A=imread('alpha/A.bmp’);
```
Where A is the variable, and in ‘alpha/A.bmp’, ‘alpha’ is the folder name and ‘A.bmp’ is the file name.

- Create a matrix of `letter` and `number` and save it in variable `NewTemplates` by using command `save(filename,variables)`.
```
%Creating Array for Alphabets
letter=[A B C D E F G H I J K L M N O P Q R S T U V W X Y Z];
%Creating Array for Numbers
number=[one two three four five six seven eight nine zero];
NewTemplates=[letter number];
```
```
save ('NewTemplates','NewTemplates')
```
#### The Template Creation code:
```
%CREATE TEMPLATES 
%Alphabets
A=imread('alpha/A.bmp');B=imread('alpha/B.bmp');C=imread('alpha/C.bmp');
D=imread('alpha/D.bmp');E=imread('alpha/E.bmp');F=imread('alpha/F.bmp');
G=imread('alpha/G.bmp');H=imread('alpha/H.bmp');I=imread('alpha/I.bmp');
J=imread('alpha/J.bmp');K=imread('alpha/K.bmp');L=imread('alpha/L.bmp');
M=imread('alpha/M.bmp');N=imread('alpha/N.bmp');O=imread('alpha/O.bmp');
P=imread('alpha/P.bmp');Q=imread('alpha/Q.bmp');R=imread('alpha/R.bmp');
S=imread('alpha/S.bmp');T=imread('alpha/T.bmp');U=imread('alpha/U.bmp');
V=imread('alpha/V.bmp');W=imread('alpha/W.bmp');X=imread('alpha/X.bmp');
Y=imread('alpha/Y.bmp');Z=imread('alpha/Z.bmp');
%Natural Numbers
one=imread('alpha/1.bmp');two=imread('alpha/2.bmp');
three=imread('alpha/3.bmp');four=imread('alpha/4.bmp');
five=imread('alpha/5.bmp'); six=imread('alpha/6.bmp');
seven=imread('alpha/7.bmp');eight=imread('alpha/8.bmp');
nine=imread('alpha/9.bmp'); zero=imread('alpha/0.bmp');
%Creating Array for Alphabets
letter=[A B C D E F G H I J K L M N O P Q R S T U V W X Y Z];
%Creating Array for Numbers
number=[one two three four five six seven eight nine zero];
NewTemplates=[letter number];
save ('NewTemplates','NewTemplates')
clear all
```

#### **Letter Detection**
Letter Detection file reads the characters from the input image and finds the highest-matched corresponding alphanumeric.

- Create a function named `letter` which gives us the alphanumeric output of the input image from class `alpha` by using command `readLetter()`.
```
function letter=readLetter(snap)
```
- Load the saved templates by using command `load NewTemplates`.
- Resize the input image so it can be compared with the template’s images by using the command `imresize(filename,size)`.
```
snap=imresize(snap,[42 24]); 
```
- Use `for`loop to correlate the input image with every image in the template to get the best match.
```
for n=1:length(NewTemplates)
    cor=corr2(NewTemplates{1,n},snap); 
    rec=[rec cor]; 
end
```
- Use `find()` command to find the index which corresponds to the highest matched character.
```
ind=find(rec==max(rec));
```
- According to that index, corresponding character is printed using `if-else` statement.

#### The Letter Detection code:
```
function letter=readLetter(snap)
load NewTemplates 
snap=imresize(snap,[42 24]); 
rec=[ ];
for n=1:length(NewTemplates)
    cor=corr2(NewTemplates{1,n},snap); 
    rec=[rec cor]; 
end
ind=find(rec==max(rec));
display(ind);
% Alphabets listings.
if ind==1 || ind==2
    letter='A';
elseif ind==3 || ind==4
    letter='B';
elseif ind==5
    letter='C';
elseif ind==6 || ind==7
    letter='D';
elseif ind==8
    letter='E';
elseif ind==9
    letter='F';
elseif ind==10
    letter='G';
elseif ind==11
    letter='H';
elseif ind==12
    letter='I';
elseif ind==13
    letter='J';
elseif ind==14
    letter='K';
elseif ind==15
    letter='L';
elseif ind==16
    letter='M';
elseif ind==17
    letter='N';
elseif ind==18 || ind==19
    letter='O';
elseif ind==20 || ind==21
    letter='P';
elseif ind==22 || ind==23
    letter='Q';
elseif ind==24 || ind==25
    letter='R';
elseif ind==26
    letter='S';
elseif ind==27
    letter='T';
elseif ind==28
    letter='U';
elseif ind==29
    letter='V';
elseif ind==30
    letter='W';
elseif ind==31
    letter='X';
elseif ind==32
    letter='Y';
elseif ind==33
    letter='Z';
    %*-*-*-*-*
% Numerals listings.
elseif ind==34
    letter='1';
elseif ind==35
    letter='2';
elseif ind==36
    letter='3';
elseif ind==37 || ind==38
    letter='4';
elseif ind==39
    letter='5';
elseif ind==40 || ind==41 || ind==42
    letter='6';
elseif ind==43
    letter='7';
elseif ind==44 || ind==45
    letter='8';
elseif ind==46 || ind==47 || ind==48
    letter='9';
else
    letter='0';
end
end
```

#### **Number Plate Detection**
Process the image and then call the above two m-files to detect the number.

- Basic commands used in above code are mentioned below:
   use `imread()` command to open the image into the MATLAB from the target folder.
   use `rgb2gray()` command to convert the RGB image into grayscale format.
   use `imbinarize()` command to Binarize 2-D grayscale image or simply we can say it converts the image into black and white format.
   use `edge()` command to detect the edges in the image, by using various methods like Roberts, Sobel, Prewitt and many others.
   use `regionprops()` command to measure properties of image region.
   use `numel()` command to calculate the number of array elements.
   use `imcrop()` command to crop the image in the entered size.
   use `bwareaopen()` command to remove small objects from binary image.

- Use `imread()` to call the input image
- Use `rgb2gray()` command to convert the input RGB image into grayscale.
- The grayscale is converted into the binary image by using  `imbinarize()`.
- Use edge()` command to detect the edge of the binary images by the Prewitt method.
Detect the location of the number plate in the entire input image using these functions:
   Use `regionprops()` command to measure properties of image region.
   Use `numel()` command to calculate the number of array elements.
```
%Below steps are to find location of number plate
Iprops=regionprops(im,'BoundingBox','Area', 'Image');
area = Iprops.Area;
count = numel(Iprops);
maxa= area;
boundingBox = Iprops.BoundingBox;
for i=1:count
   if maxa<Iprops(i).Area
       maxa=Iprops(i).Area;
       boundingBox=Iprops(i).BoundingBox;
   end
end  
```
-crop the number plate and remove the small objects from the binary image by using command ``imcrop()` and `bwareaopen()` respectively.
-Then, the below code is used to process that cropped license plate image and to display the detected number in the image and text format (in the command window).
#### The Number Plate Detection code:
```
close all;
clear all;

im = imread('Number Plate Images/image1.png');
imgray = rgb2gray(im);
imbin = imbinarize(imgray);
im = edge(imgray, 'prewitt');

%Below steps are to find location of number plate
Iprops=regionprops(im,'BoundingBox','Area', 'Image');
area = Iprops.Area;
count = numel(Iprops);
maxa= area;
boundingBox = Iprops.BoundingBox;
for i=1:count
   if maxa<Iprops(i).Area
       maxa=Iprops(i).Area;
       boundingBox=Iprops(i).BoundingBox;
   end
end    

im = imcrop(imbin, boundingBox);%crop the number plate area
im = bwareaopen(~im, 500); %remove some object if it width is too long or too small than 500

 [h, w] = size(im);%get width

imshow(im);

Iprops=regionprops(im,'BoundingBox','Area', 'Image'); %read letter
count = numel(Iprops);
noPlate=[]; % Initializing the variable of number plate string.

for i=1:count
   ow = length(Iprops(i).Image(1,:));
   oh = length(Iprops(i).Image(:,1));
   if ow<(h/2) & oh>(h/3)
       letter=Letter_detection(Iprops(i).Image); % Reading the letter corresponding the binary image 'N'.
       noPlate=[noPlate letter] % Appending every subsequent character in noPlate variable.
   end
end
```
## Environment Variables
To run this project, the administrator will need to add the following environment variables to their software file
- add the image of the vehicle in the Number Plate Detection file by adding the name of the image and the location the image in line 4, after imread function.
```
im = imread('location of the image/image name.png');
```
## Programmers
- Ruba Khawfani
- Shefaa SHalabi
- Rahaf Alharthi
Footer
© 2022 GitHub, Inc.
Footer navigation
Terms
Privacy
