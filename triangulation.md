### Triangulation
<img src="images/triangulation-0.jpg" width="50%">
Note: Of course this does not help us much. We need an algorithm that really does the encryption. So how can we force it to do what we want by test? Currently we have only this one test. It provides our production code a large variety of how to act. Currently our production code could also raise a database or connect to a server in the internet. We need to center its freedom to what we need it to do. Just similar to how triangulation in GPS is working.

??HORIZONTAL
### One Satellite
<img src="images/triangulation-1.jpg" width="50%">
Note: If you receive only one Satellite, you might be somewhere in the wide circle that the signal of the satellite projects on the surface of earth. 

??HORIZONTAL
### Two Satellites
<img src="images/triangulation-2.jpg" width="50%">
Note: If you receive two Satellites, both projected circles intersect in two points. Both might represent your position on earth.

??HORIZONTAL
### Three Satellites
<img src="images/triangulation-3.jpg" width="50%">
Note: Only if you you receive three or more Satellites your position can be determined. 

So we need also further tests to determine the concrete behaviour of our production code. The more tests we write, the closer we get to the specification
