---
layout: article
title: "About HerdNerds"
date: 
modified:
excerpt:
image:
  feature:
  teaser:
  thumb:
ads: false
comments: true
categories: About
---
HerdNerds is a group of electronics and robotics enthusiasts at Playtika Santa Monica.

This is a test of math: 300 $\Omega$

And an equation:

$$x = \sum_{i=1}^n \left( \frac{x^2}{y} \right)$$

{% highlight c linenos %}
// set this to the first pin where wiring starts.
int ledStart = 2;
int digitStart = 10;

// set to true if the common pin is HIGH, false otherwise
boolean commonHigh = false;


void setup() {
  // Enable all A-G and DP and outputs, 
  // set them to OFF (if common is high, then 1 is off).
  for(int i=0;i<8;++i) {
    pinMode(ledStart + i, OUTPUT);
    digitalWrite(ledStart + i, commonHigh?1:0);
  }
  for(int j=0;j<4;++j) {
    pinMode(digitStart + j, OUTPUT);
    digitalWrite(digitStart + j, commonHigh?0:1);
  }
}

void loop() {
  // write the number 0 - F (hex)
  // onto the display each half second
//  for (int j=0; j<4; j++) {
//    digitalWrite(digitStart + j, commonHigh?1:0);
//    for(int i=0;i<16;i++) {
//      writeDigitToDisplay(i);
//      delay(500);
//    }
//    delay(500);
//    digitalWrite(digitStart + j, commonHigh?0:1);
//  }
  writeDigitToDisplay(4);
  digitalWrite(digitStart, commonHigh?1:0);  
  delay(1);
  digitalWrite(digitStart, commonHigh?0:1);

  writeDigitToDisplay(3);  
  digitalWrite(digitStart + 1, commonHigh?1:0);
  delay(1);
  digitalWrite(digitStart + 1, commonHigh?0:1);


  writeDigitToDisplay(2);  
  digitalWrite(digitStart + 2, commonHigh?1:0);
  delay(1);
  digitalWrite(digitStart + 2, commonHigh?0:1);

  writeDigitToDisplay(1);  
  digitalWrite(digitStart + 3, commonHigh?1:0);
  delay(1);
  digitalWrite(digitStart + 3, commonHigh?0:1);
}



void writeDigitToDisplay(int digit) {
  // Now we define the bits that are on and off
  // for each segment, These are used in the
  // function below to turn the right bits on.
  int dig[16] = {
  // bits     6543210
  // digits   abcdefg
          0b1111110,//0
          0b0110000,//1
          0b1101101,//2
          0b1111001,//3
          0b0110011,//4
          0b1011011,//5
          0b1011111,//6
          0b1110000,//7
          0b1111111,//8
          0b1111011,//9
          0b1110111,//a
          0b0011111,//b
          0b1001110,//c
          0b0111101,//d
          0b1001111,//e
          0b1000111 //f
  };
  
  
  // iterate through each bit
  for(int i=0;i<7;++i) {
    // isolate the current bit in the loop.
    int currentBit = (1<<(6-i));
    // and compare with that bit in the digit
    // definition above.
    int bitOn = (currentBit&dig[digit])!=0;
    
    // if common is high we invert the logic, as 0 is on.
    if(commonHigh) {
      bitOn = !bitOn;
    }
    
    // and lastly set the bit
    digitalWrite(ledStart+i, bitOn);
  }
}
{% endhighlight %}
