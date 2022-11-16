---
title: Unit testing vs Integration testing
author: Ruslan Bes
type: post
date: 2015-05-12T13:01:30+00:00
url: /2015/05/12/unit-testing-vs-integration-testing/
dsq_thread_id:
  - 5215674916
categories:
  - Software engineering

---
**Unit testing** is more or less like the [tire testing][1]. You take a wheel, disconnect it form the car, put it on the stand and run for a couple of hours. If you figured out the right parameters for the test (how long to run a wheel and how many pressure to apply) then you can be sure than it will work on the real car.

**Integration testing** or end-to-end testing is more like a neurologist hitting your knee with a [hammer][2] to test the reflexes or like the live test-drive of a new car. So you test an actual system without disconnecting the part.

When [something terrible happens][3] to your system, the unit tests helps you to exclude some parts that are working for sure. The integration tests on the other hand do not help much here but they reduce the chance of firing a bug in the most common use-cases.

 [1]: https://www.youtube.com/watch?v=nmo_dkNZIHM
 [2]: http://image.shutterstock.com/display_pic_with_logo/604408/604408,1299445949,2/stock-photo-the-neurologist-testing-knee-reflex-on-a-female-patient-using-a-hammer-72596347.jpg
 [3]: http://media-cache-ec0.pinimg.com/736x/e7/5f/6a/e75f6a891d303562c9160f9ecec3d36e.jpg