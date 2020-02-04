## From Sensor Noise To Allan Deviation

### What is noise?

Every sensor has a noise, it's only a matter of how "big" the value is for different sensors. For an accelerometer, the data you measured directly from it is the composition of actual real measurement,
noise, bias etc. If we take one measurement from an acc, without other information such as noise density, you have absolutely no way to find out what the real measure is from the measured data.

One property of noise is, over a period of time that is long enough, the sum of noise will be zero.

So, if we'd like to measure the real value of current acceleration of a accelerometer that is put still on a table, how can do do that?

One thing we can do is to take a lots of measurements of the acclerometer and average them, then the averaged value will be very close to the real value if each measurement is composed of only real value and 
noise. If we take, for example, 10 million measurements of and accelerometer and plot the value of Z axis, we will get a graph where the X axis represents measurement index and Y axis represents the total measurement. 

The value in Y axis will oscillate around the gravity of 9.8 or -9.8 depending on the definition of the Z axis, and the spread of each measurement is always within a tiny range.

So, using abovementioned approach we can measure the "true" gravity value given it is put still on a table, but we don't want to measure the gravity, instead we would like to measure the actual acceleration of
a moving object, such as a robot or drone.

### Allan Variance

One way to characterize a sensor is using measurement variance.

First of all, consider we have 2 million measurements of a mems IMU, and we only look at the acceleration, if we calculate the variance from every single measurement, the variance is equal to the sensor noise.

For this scenario, we are using a group of size 1, if we increase the group size to N, in another words, we firstly divide all the measurements into small groups of size N, and we calculate the variance from each
group, there should be a value N when we can get a minimal variance, and the minimal variance is bias.

Now, what if we continue increasing the group size, for example, let N equal to 10 million. Most likely the variance we calculated will be greater than the minimal variance that we just calculated. The new variance is impacted by Low Frequency Noise, which is usually the sum of multiple factors such as temperature, vibrational noise and random walk.

So, to sum up, if we start from a group size of 1 and increase the group size, the variance will firstly start to decrease, and increase before it dropped to a minimum area.

Take this graph as an example:


<img src="https://github.com/gaowenliang/imu_utils/blob/master/figure/gyr.jpg" align="right" width="300px" alt="hardware">


Usually we would take of log of each axis and instead of using variance, we would use deviation for the Y axis.

This graph can be divided into 4 parts:

#### A: Single Point Noise / Quantization Noise

This value can help to tell how much error from noise a single measurement point will have.

It matters when we are developing an algorithm that uses every IMU measurement, such as IMU Euler Integration of Mid Point Integration.

#### B: Improvment From Averaging

The deviation value should decrease when group size increases, and if tell us about the amount of noise a small group of measurements have.

It matters when we are using an algorithm that uses a group of IMU measurements, such as RK4 or RK5.

#### C: Bias Instability

The group size conresponding to the minimal deviation.

It is helpful to compare the performance of different sensors in the best case of scenario.

#### D: Low Frequency Noise / Random Walk

This increase is caused by random walk, it will converge to zero given a large enough group size, but it would take a long time.

Usually the price of a sensor is closely related to the low frequency characteristic.


### Further Reading

http://cache.freescale.com/files/sensors/doc/app_note/AN5087.pdf
