Deploy Backend Instances
------------------------

Launch Google Chrome and click on the “Login – OpenStack…” bookmark.

    |image5|

    |image6|

Login to Horizon (OpenStack GUI) using the username: demo, password:
demo

    |image7|

You should see.

    |image8|

Click on “Instances” and then “Launch Instance” (top right of page).

    |image9|

For the Instance name specify “server” for the count enter “2”. Then
click next.

    |image10|

Click on the “+” next to “f5demo”.Then click next.

    |image11|

Click on the “+” next to “m1.tiny”. Then click next.

    |image12|

Click on the “+” next to “internal” Network. Then click on next TWICE
until you are on the Security Groups tab.

    |image13|

On the Security Groups tab click on the “+” next to “default-allow-all”.
Then click next.

    |image14|

Click on the “+” next to “demo-key-pair” and the click on “Launch
Instance”.

    |image15|

You should now see them starting.

    |image16|

Clicking on “server-1” then “Log” you should see

    |image17|

In this lab environment OpenStack is running in a nested environment.
This will cause the

.. TODO:: sentence above is incomplete

.. |image5| image:: /_static/image7.png
.. |image6| image:: /_static/image8.png
.. |image7| image:: /_static/image9.png
.. |image8| image:: /_static/image10.png
.. |image9| image:: /_static/image11.png
.. |image10| image:: /_static/image12.png
.. |image11| image:: /_static/image13.png
.. |image12| image:: /_static/image14.png
.. |image13| image:: /_static/image15.png
.. |image14| image:: /_static/image16.png
.. |image15| image:: /_static/image17.png
.. |image16| image:: /_static/image18.png
.. |image17| image:: /_static/image19.png
