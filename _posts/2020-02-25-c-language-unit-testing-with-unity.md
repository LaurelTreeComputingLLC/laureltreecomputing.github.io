---
layout: post
title: C Language Unit Testing with Unity
description: Unit Testing with Unity in C when writing your own unit tests aren't
  as quality
image: 
category: 
tags: 
date: 2020-02-25 20:51 -0700
---
# Unit Testing

I won't go into detail about Unit Testing. But in my own words it means testing each individual module, class, function, or piece of code individually and self-contained for flaws, bugs, or the lack thereof.

Unit Tests play a major role in [TDD](https://en.wikipedia.org/wiki/Test-driven_development). TDD(Test-Driven Development) is really big in the software development world so much so that Agile practices promote TDD as well as SCRUM and other methodologies.  TDD on its own is very powerful.  Still, there still exist many conversations about the pros and cons of TDD. 

 Some coders says it slows them down, others say that you benefit from the tests being developed and maintained along the way; the code comes out more robust and stable.  

I don't take a stand on this topic but just choose to learn either way.

Again without too much of a description TDD is:

* A programmer starts his/her coding by writing the tests for some code/module/etc
* Running the test presumably fails
* The programmer fills in the missing code that the tests rely on
* The tests ideally Pass
* The programmer repeats the proces each step of the way until a program is ready for the public

# Unity
Unity is a framework written in C and meant for C and C++ applications offered and developed by The [ThrowTheSwitch.org ](http://www.throwtheswitch.org) group.  They provide the [Unity Framework](http://www.throwtheswitch.org/unity) as open source along with other projects to make testing and coding much easier.  I intend to use [CMock](http://www.throwtheswitch.org/cmock) a Mock Framework and [CException](http://www.throwtheswitch.org/cexception) an exception functional for the C language( using setjmp and longjmp) later and I'll write about both eventually.

Unity includes many MACROS and functions to help develop swift, easy and automated code testing.

Briefly I go over what i've used and learned.

To use unity include the header in your test file ideally before any other headers are included:

```
 #include "unity.h"
 #include "my_set.h"
 ```

Make sure the file is in your include path.

You'll need to define a couple of functions:

```
void setUp(void){}
void tearDown(void){}
```

Inside each of these functions you enter code that will be run before and after each individual test.

```
my_set* set;_
void setUp(void){
	set = malloc(sizeof(*set));
}
void tearDown(void){
	free(set);
	set = NULL;
}
```

Write tests as void func(void) functions.  The name of the test should start with the word test:

``` 
void test_add_item_to_my_set(void){
	/* do something cool */

	add_integer_to_my_set(my_set);
}
```


In the main(void) function(you won't usually be needing the command-line arguments but probably can).

Initialize Unity at the beginning of main with UNITY_BEGIN();_

return the number of failed tests with UNITY_END();_

```
int main(void){
	UNITY_BEGIN();
	return UNITY_END();
}
```

You will run tests inside the main function with a RUN_TEST() macro, argument should be the name of the test function:

```
RUN_TEST(some_test_func);
```

Run the tests via this macro between the UNIT_BEGIN and UNITY_END:
```
int main(void){
	UNITY_BEGIN();
	RUN_TEST(test_add_item_to_my_set);
	return UNITY_END();
}
```

Run more tests:

```
#include "unity.h"
#include "file_header_needed.h"

void setUp(void){
	/* setup */
}

void tearDown(void){
	/* tear it down */
}

void test_this_feature(void){
}
void test_that_other_feature(void){
}

int main(void){
	UNITY_BEGIN();
	RUN_TEST(test_this_feature);
	RUN_TEST(test_that_other_feature);
	return UNITY_END();
}

```
# Runner Helper script Generate_Test_Runner.rb
With a little help from a helper script in the auto folder 'generate_test_runner.rb' and ruby, you can forget about the whole main function and setup/teardown's if you don't need them.

By supplying only the test functions for your code and necessary headers included, the script will automatically create a C file that will run each and every test for you. It creates the main function with all the macro invocations and you only have to compile it together with your test and cod you are testing.

> computer:src_folder user$ ruby generate_test_runner.rb path_to_test_file.c

per the example i've been using:

> computer:src_folder user$ ruby generate_test_runner.rb my_set_tests.c

will generate the file ```my_set_tests_runner.c ```

> computer:src_folder user$ gcc -o test_runner my_set_class.c my_set_tests.c my_set_tests_runner.c

> computer:src_folder user$ ./test_runner