pypus
=====

https://github.com/wilsonfreitas/pypus

Imagine the situation where you need to pass of a series of numbers contained in a file through a series of transformations like absolute, logarithm, square power, cubic power and so on. Well, you think:

- Wow, I need to open my editor write a program to read a file, put its content into a numeric array structure, take the series of functions and write the results.

If your file has some tabular format you still need to split each line and coerce the fields into a numeric matrix data structure. This task becomes a pain when you have to do a lot of transformations to generate graphics or more data for analysis and you appear with a set of ten small files to maintain. It usually happens for those that work with scientific modeling and data processing, analyzing and generating data through simulations.

Ok, you can use awk and solve this with an one line command. awk will split the rows in fields and has a small set of mathematical functions. But, if you need a more complex function like Fourier transform, forget.

Further, its very common for those that work with data processing to use gnuplot to create graphics. I will show to you a situation that happens to me very frequently. I study time series and use gnuplot to watch the data I am working. When I look at a time series I like to see its logarithm and take the first difference, it's a very usual transformation known as logarithmic return.

    gnuplot> plot "< awk 'BEGIN{a = \"S\"} {
    if (a == \"S\") a = log($1); 
    else print log($1) - a;
    }' timeseries.txt" w l

Ok, awk solves my problem, but I don't like awk, I can't control the way the text is converted to number and I don't trust in its precision. Awk has not scientific purposes. I can write a program using my preferred language to read the time series file and send the results to standard output, it also solves my problem, but it is also usual to me to take the first difference of the data after the log, and what do I do now ? I can write another program, actually I can write many programs is possible to fulfill my requirements. I can write programs in C, C++, Python ... I will have dozens programs and all my issues will be closed. But, from my experience, I know that many programs are very hard to maintain, specially in my case where I have small and specific programs, imagine the situation where I need to handle a new data format, wow, it will be a pain to update, not all, the most used programs to handle it.

It's enough, I write pypus to have time to worry with interesting problems and in my case the interesting problems are those related with data modeling. I create pypus having in mind that:

  it must be simple and straight forward
  it must to use existing APIs (I don't want to write another fft implementation)
  it must be quickly extensible 

Pypus is a tool to execute one-liners commands for data handling and you can use the well established scientific APIs developed for the Python language:

  scipy
  numpy
  matplotlib 

Oh, pypus is also developed in Python, just to mention.

Examples
--------
Exponential of a time series

using awk

    $ awk '{print exp($1)}' dataseries.txt

using pypus

    $ pypus '@numpy.exp' dataseries.txt

Logarithmic returns of a time series

using python

    $ python logreturn.py dataseries.txt

using pypus

    $ pypus '@numpy.log @numpy.diff' dataseries.txt

Generating random numbers

using awk

    $ awk 'BEGIN{ for (i=0 ; i<100 ; i++) print rand()}'

using pypus

    $ pypus -g '@numpy.random(100)'

* Use http://www.dabeaz.com/ply/ to build the interpreter.
