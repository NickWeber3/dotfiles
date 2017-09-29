Overview of Design Decisions

Whenever I start building a program, one of my primary objectives is to make my code as abstract as I can. In other words, I want to manage the complexity of the program, usually by seperating the individual components of the project into seperate functions, resulting in a relatively short main() function that is easy to interpret. This makes programs much easier for others to read on first and repeat viewings, and much more comprehensive. 

In this project, I was able to do this quite effectively:
  -	I originally broke the program down into 4 parts: 
  
            1) Create input stream to the input file
            
            2) Read in data one line at a time, each line will contain one of three commands
            
            3) Change the data in the database according to the command. If the command is "Add", first do a Luhn 10 check
              
            4) After all data is read, display account summaries to STDOUT
            
  -	I noted that although the method for reading in the file may be different (command line argument or STDIN), the method for processing the data from the file would always be the same. As a result, I created the update_accounts() function to process the file data. update_accounts() takes an input stream as an input argument, and abstractly reads in the data from whatever stream is passed into the function. This way, the file can be read from any input stream and the data can still be processed using the same lines of code.
  -	I wrote a seperate function to run a Luhn 10 validation on each card number added to the database. I wrote my own based off of the research I did on the algorithm because C++ does not have a library containing it. The function, valid_luhn_10(), is only called in update_accounts(), and only when the "Add" command is processed.
  -	 I also wrote a seperate function to print the account summaries at the end of the program, print_summary(), in order to conserve space in the main() function. Like the objective above states, this makes reading through the code much easier to someone unfamiliar with it.

The next project decision I was faced with was choosing a data structure to act as the database for the accounts. I started by considering either a hash table (unordered_map in C++) or a map. Both of these structures store data in key:value pairs, which is ideal for storing data linked to a name or individual ("name:data" key:value pairs). The benefits of a hash table include O(1) expected time complexity for insertion, search, and delete, whereas a map has O(log(n)) complexities for all of those operations. However, I decided to go with a map structure because:
  -	When printing out a summary of the accounts at the end of the program, a map will have stored the key:value pairs in alphabetical order automatically, making outputting the summaries in alphabetical order according to names simple and possible in O(n) time. If I had used a hash table, I would have had to first sort the hash table and then output the data, requiring more code and a worse time complexity.
  -	 Although a hash table has an expected O(1) time complexity for insertion, lookup, and deletion, it also has a worse case O(n) time complexity for those operations as well. A map always performs these same operations in O(log(n)) time, so it still beats the hash table in worse case scenarios.

In addition to these, some smaller design decisions I implemented include:
-	The Luhn 10 algorithm requires isolation and arithmatic on the individual digits in a credit card's up-to-19 digits card number. In order to do this, I pass the card number as a string object into my valid_luhn_10() functionbecause, in C++, accessing characters in a string is much easier than accessing digits in a number. I then performedsome simple ASCII arithmatic to transform each digit-character to its proper digit-integer representation.
-	My program immediately erases the '$' character from each dollar amount read in. Since the dollar amount is originally read in as a string object, I then also immediately convert each dollar amount into an integer using C++'s stoi() function from its <string> library. Both of these actions then allow the dollar amount to be used in  arithmatic operations.





Why I picked C++

Although I am equally familiar with both C++ and Python, I almost always choose C++ when I have the choice between the two. The thing I enjoy the most about C++, strangely, is that your code must be very explicit in order for what you want to occur to actually occur. For example, a for-loop in C++, i.e. for (int i =0; i < 100; ++i), carries much more information than a Python for-loop, i.e. for item in items. In the C++ version, I can actually see all the inner-workings of the loop, what is being incremented, how many times it is being incremented, and how much it is being incremented by (or decremented by). When I use the Python version, it almost seems like magic, and I have no idea how the for-loop is actually doing what it is doing. I do see why lots of programmers prefer Python's simplicity to C++'s drawn out, convoluted syntax, but I enjoy having a deeper conceptual understanding of how my code is actually working on the page in front of me. That is why whenever I have the choice, I choose C++.





Running, compiling, testing

Some of my code, specifically the print_summary() function, uses a C++11's range based for-loop. I did this to make using iterators much cleaner and easier, but it requires the C++11 flag to be present during compilation. As a result, compile theprogram with this command in the command line or terminal:

    g++ -std=c++11 credit_card_processing.cpp -o processor

Then you can run the program, including a filename passed in as a command line arguments or from STDIN, using this command:

    ./processor input.txt            OR            ./processor < input.txt

The test files I made are named:
    
    test_input_1.txt
    test_input_2.txt
    test_input_3.txt

For automated testing, I wrote and included a shell script 'run_tests.sh'. To automatically run my tests run the following commands in sequence from the command line or terminal:

    g++ -std=c++11 credit_card_processing.cpp -o processor
    bash run_tests.sh

All test output will then be inside of a newly created text file named "all_test_output.txt", housed in the same folder as the program and shell script!




