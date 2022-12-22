---
title: Development
author: a.whit ([email](mailto:nml@whit.contact))
date: December 2022
---

<!-- License

Copyright 2022 Neuromechatronics Lab, Carnegie Mellon University (a.whit)

Created by: a. whit. (nml@whit.contact)

This Source Code Form is subject to the terms of the Mozilla Public
License, v. 2.0. If a copy of the MPL was not distributed with this
file, You can obtain one at https://mozilla.org/MPL/2.0/.
-->

This document contains notes about the development of this package.

### Abstraction versus replication

Using abstraction to reduce the amount of replicated code fragments is often a 
good [practice][dont_repeat_yourself]. It can promote encapsulation and 
facilitate changes. However, abstraction can encourage premature optimization 
and unnecessary complexity. At present, there is some uncertainty about the 
amount of abstraction that is appropriate for this package.

A hybrid approach will be adopted, until this question is resolved. Some of the 
code will be more abstract (e.g., the 
[server](ros_spheres_environment/server.py)), whereas others will be more 
direct and repetitive (e.g., the [client](ros_spheres_environment/client.py)).
Experience will dictate which approach is more effective, in this context.

### Data field size

It is sometimes desirable to attempt to minimize the total size of data stores 
(e.g., bag files) by reducing the size of data structure (i.e., ROS2 message) 
fields to only the number of bits that are strictly necessary. For example, a 
64 bit floating point number might be constrained to 32 bits if the latter 
precision is sufficient for representing most of the data. If data are recorded 
for an hour at 1000 Hz, then the latter field size demands only 13.73 Mb of 
storage space, whereas the former would require 27.47 Mb.

However, it is sometimes not advisable to reduce field size. This is reflected 
in the message interface definition for the [geometry_msgs/Point32] message, 
which recommends using the (larger) [geometry_msgs/Point] message whenever 
possible. Anecdotally, this has been observed to have a meaningful effect for 
[example_interfaces/Float32] messages and Python floats. A Float32 message 
assigned the value `0.1` can sometimes be received with imprecise values like 
`0.10000000149011612` on the far end. Messages sent using the 
[example_interfaces/Float64] message, on the other hand, do not suffer from the 
same mismatch.


<!---------------------------------------------------------------------
   References
---------------------------------------------------------------------->

[dont_repeat_yourself]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

[doctest]: https://docs.python.org/3/library/doctest.html

[setuptools]: https://setuptools.pypa.io/en/latest/userguide/quickstart.html#basic-use

[neuromechatronics]: https://www.meche.engineering.cmu.edu/faculty/neuromechatronics-lab.html

[pip install]: https://pip.pypa.io/en/stable/cli/pip_install/

[pytest]: https://docs.pytest.org/

[unittest]: https://docs.python.org/3/library/unittest.html

[geometry_msgs/Point]: https://docs.ros2.org/galactic/api/geometry_msgs/msg/Point.html

[geometry_msgs/Point32]: https://docs.ros2.org/galactic/api/geometry_msgs/msg/Point32.html

[example_interfaces/Float32]: https://docs.ros2.org/galactic/api/example_interfaces/msg/Float32.html

[example_interfaces/Float64]: https://docs.ros2.org/galactic/api/example_interfaces/msg/Float64.html

