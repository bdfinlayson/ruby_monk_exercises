# ruby_monk_exercises
Collection of my solutions to problems taken from rubymonk.com

Find non-duplicate values in an Array
------------------------------------------------------------
####Problem Statement

Given an Array, return the elements that are present exactly once in the array.

You need to write a method called non_duplicated_values to accomplish this task.

####Example: 
Given [1,2,2,3,3,4,5], the method should return [1,4,5]

####Solution:
  
    def non_duplicated_values(values)
      values.select { |x| values.count(x) == 1 } #you can also use the `find_all` method in place of `select`
    end
    
    #=> returns [1,4,5], given [1,2,2,3,3,4,5]
    #=> returns [1,3], given [1,2,2,3,4,4]

Check if all elements in an array are Fixnum
-----------------------------------------------------------------
####Problem Statement
Given an array, return true if all the elements are Fixnums.

You need to write array_of_fixnums? method to accomplish this task.

####Example:
Given [1,2,3], the method should return true

####Solution

    def array_of_fixnums?(array)
      array.all? { |x| x.is_a? Fixnum }
    end

#####An alternative solution might be:
  
    def array_of_fixnums?(array)
      y = array.find_all { |x| x.is_a? Fixnum }
      y.size == array.size ? true : false
    end
  
Kaprekar's Number
------------------------
####Problem Statement

9 is a Kaprekar number since 
9 ^ 2 = 81 and 8 + 1 = 9

297 is also Kaprekar number since 
297 ^ 2 = 88209 and 88 + 209 = 297.

In short, for a Kaprekar number k with n-digits, if you square it and add the right n digits to the left n or n-1 digits, the resultant sum is k. 
Find if a given number is a Kaprekar number.

####Solution:
  def kaprekar?(k)
    no_of_digits = k.to_s.size
    square = (k ** 2).to_s
    
    second_half = square[-no_of_digits..-1]
    first_half = square.size.even? ? square[0..no_of_digits-1] : square[0..no_of_digits-2]
    
    k == first_half.to_i + second_half.to_i
  end
  
    #=> returns true for 9
    #=> returns false for 46
    #=> returns true for 55
    #=> returns true for 297
    #=> returns false for 90
  
Enough Contrast?
-------------
####  Problem Statement
For 2 Colors in RGB: 
(R1, G1, B1) and (R2, G2, B2),

Brightness index is: 
( 299*R1 + 587*G1 + 114*B1) / 1000 

Brightness difference is: 
Absolute difference in brighness indices 

Hue difference is: 
|R1 - R2| + |G1 - G2| + |B1 - B2|
where |x| is the absolute value of x

If Brightness difference is more than 125 and the Hue difference is more than 500 then the colors have sufficient contrast

Find out if the given color combos have sufficient contrast and get all the tests passing.

####Solution:

  class Color
    attr_reader :r, :g, :b
    def initialize(r, g, b)
      @r = r
      @g = g
      @b = b
    end
  
    def brightness_index
      (r*299 + g*587 + b*114) / 1000
    end
  
    def brightness_difference(another_color)
      (brightness_index - another_color.brightness_index).abs
    end
  
    def hue_difference(another_color)
      (r-another_color.r).abs +
      (g-another_color.g).abs +
      (b-another_color.b).abs
    end

    def enough_contrast?(another_color)
      brightness_difference(another_color) > 125 && hue_difference(another_color) > 500
    end
  end
  
    initializes the color with the RGB values 
    returns the correct brightness index for (42, 21, 58) 
    returns the correct brightness index for (100, 200, 255) 
    returns the correct brighness difference between 2 colors 
    returns the correct hue difference between 2 colors 
    tells that there is is not enough contrast between (42, 21, 58) and (240, 41, 25) 
    tells that there is is enough contrast between (42, 42, 42) and (210, 210, 210)
  
Time to run code
------------------
####Problem Statement

You are given some code in the form of lambdas. 

Measure and return the time taken to execute that code. 

You may use Time.now to get the current time.

####Solution:

    def exec_time(proc)
      begin_time = Time.now
      proc.call
      Time.now - begin_time
    end
    
    #=> takes more time to execute a task 10 times 
    #=> division takes more time than addition 
    #=> Array::find takes more time than Array::[] 
  
Number shuffle
----------
####Problem Statement

Given a 3 or 4 digit number with distinct digits, return a sorted array of all the unique numbers that can be formed with those digits.

Example: 
Given: 123 
Return: [123, 132, 213, 231, 312, 321]

####Solution:

    def number_shuffle(number)
      no_of_combinations = number.to_s.size == 3 ? 6 : 24
      digits = number.to_s.split(//)
      combinations = []
      combinations << digits.shuffle.join.to_i while combinations.uniq.size!=no_of_combinations
      combinations.uniq.sort
    end
    
    #=> returns [123, 132, 213, 231, 312, 321] for 123 ✔
    #=> returns [1234, 1243, 1324, 1342, 1423, 1432, 2134, 2143, 2314, 2341, 2413, 2431, 3124, 3142, 3214, 3241, 3412, 3421, 4123, 4132, 4213, 4231, 4312, 4321] for 1234
