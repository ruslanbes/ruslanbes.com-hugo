---
title: Price is not a double
author: Ruslan Bes
type: post
date: 2017-08-03T18:49:50+00:00
url: /2017/08/03/price-is-not-a-double/
dsq_thread_id:
  - 6038962377
categories:
  - Java
tags:
  - floating-point
  - price

---
<img loading="lazy" class="aligncenter size-full wp-image-868" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/08/hotprice.png?resize=375%2C360&#038;ssl=1" alt="Price" width="375" height="360" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/08/hotprice.png?w=375&ssl=1 375w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/08/hotprice.png?resize=300%2C288&ssl=1 300w" sizes="(max-width: 375px) 100vw, 375px" data-recalc-dims="1" />

When a software developer sees a requirement to store prices and do price calculations he has to decide which type he&#8217;s going to use.

<div>
  <table style="margin: auto;" border="1" summary="Default values for primitive types">
    <tr>
      <th id="h1" align="left">
        <strong>Data Type</strong>
      </th>
      
      <th id="h2" style="text-align: center;" align="left">
        <strong>Default Value (for fields)</strong>
      </th>
    </tr>
    
    <tr>
      <td headers="h1">
        byte
      </td>
      
      <td headers="h2">
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        short
      </td>
      
      <td headers="h2">
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        int
      </td>
      
      <td headers="h2">
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        long
      </td>
      
      <td headers="h2">
        0L
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        float
      </td>
      
      <td headers="h2">
        0.0f
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        double
      </td>
      
      <td headers="h2">
        0.0d
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        char
      </td>
      
      <td headers="h2">
        &#8216;\u0000&#8217;
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        String (or any object)
      </td>
      
      <td headers="h2">
        null
      </td>
    </tr>
    
    <tr>
      <td headers="h1">
        boolean
      </td>
      
      <td headers="h2">
        false
      </td>
    </tr>
  </table>
</div>

Since price is usually explained in Euros (Dollars, Hryvnas, Bitcoins) it has an integer part and a decimal part. So the natural choice is to skip `int` and take `double` or `float`.

This, however, has some consequences that are often learned the hard way. If you already forgot CS 101 here is a [reminder what is wrong with doubles][1].  
If you draw possible value spaces, they will look like this:

<img loading="lazy" class="aligncenter size-full wp-image-872" src="https://i2.wp.com/ruslanbes.com/devblog/rbes-content/uploads/2017/08/numbers-1.png?resize=464%2C427&#038;ssl=1" alt="numbers" width="464" height="427" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/08/numbers-1.png?w=464&ssl=1 464w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2017/08/numbers-1.png?resize=300%2C276&ssl=1 300w" sizes="(max-width: 464px) 100vw, 464px" data-recalc-dims="1" /> 

so the space of doubles is much bigger than the space of prices. That means when you do calculations with doubles you may get **something that is not a price**.

## Example

First, an [easy example][2] that starts challenging your belief in math:

<pre class="brush: java; title: ; notranslate" title="">System.out.println(8.97);
		System.out.println(8.97 + 4.95 - 4.95);
</pre>

The result is not as you have expected:

<pre class="brush: bash; title: ; notranslate" title="">8.97
8.970000000000002
</pre>

This one is pretty clear but what if your task is to make some more calculations. Suppose you have the following problem statement:

> Calculate discount between old and new price and show as a percentage without decimal signs.  
> Additional requirement: always round percentage down.

### Solution that is about almost correct

<pre class="brush: java; title: ; notranslate" title="">public int getPriceDifferenceInPercentage(Double oldPrice, Double price) {
    return new BigDecimal((oldPrice - price) * 100 / oldPrice)
        .setScale(0, BigDecimal.ROUND_DOWN).intValue(); 
}</pre>

The code looks innocent enough. We do discount calculation, convert to BigDecimal, round down. [What could possibly go wrong][3]?

Well, it turns out 14.95 &#8211; 8.97 in doubles is not 5.98 but 5.979999999999999 and it goes down from there because we round it down.

### Correct solutions

The first solution would be to immediately convert the price from `double` to `BigDecimal` with scale 2 and do calcluation is BigDecimals. This makes the code a little bulky but solves the problem:

<pre class="brush: java; title: ; notranslate" title="">public int getPriceDifferenceInPercentage(Double oldPrice, Double price) {
    // Convert doubles to decimals with the scale of 2 to make a precise calculation. 
    BigDecimal oldPriceDecimal = new BigDecimal(oldPrice)
        .setScale(2, BigDecimal.ROUND_HALF_UP);
    BigDecimal priceDecimal = new BigDecimal(price)
        .setScale(2, BigDecimal.ROUND_HALF_UP);
    BigDecimal percentage = oldPriceDecimal
        .subtract(priceDecimal)
        .divide(oldPriceDecimal, 2, BigDecimal.ROUND_DOWN);
    return percentage.multiply(new BigDecimal(100)).intValue();
}
</pre>

[Test on ideone.][4]

The second approach is to do all the computations in doubles but do rounding with 1 decimal digit before converting it to the end result. This will often be enough to eliminate the imprecision you got during calculations.

<pre class="brush: java; title: ; notranslate" title="">public int getPriceDifferenceInPercentage(Double oldPrice, Double price) {
    return new BigDecimal((oldPrice - price) * 100 / oldPrice).setScale(1, BigDecimal.ROUND_HALF_UP).intValue();
    //                                                                  ^^ fix imprecision problem ^^  
}
</pre>

[Test on ideone.][5]

Lesson to learn: price is not a double. Use precise numeric types when you&#8217;re working with it.

 [1]: http://floating-point-gui.de/
 [2]: https://ideone.com/33cmo7
 [3]: http://ideone.com/8l6UNw
 [4]: http://ideone.com/eyVFB5
 [5]: http://ideone.com/LVwMYM