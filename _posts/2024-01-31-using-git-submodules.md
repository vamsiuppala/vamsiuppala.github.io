First go to the repo of your interest. I am going to create a new repo in Github just to try this out.

<img src="/images/2024-01-31-using-git-submodules/image1.png" style="width:5.03529in;height:6.90793in" />

Clone it into your desktop:

<img src="/images/2024-01-31-using-git-submodules/image2.png" style="width:6.5in;height:2.92847in" />

Use the command line to navigate to the folder where you want to close this repository to and use the following command:

\$ git clone <https://github.com/vamsiuppala/try-submodules.git>

Navigate into the repository to add a submodule that you would like to use. I am going to try using the popular Python library - [requests](https://github.com/psf/requests?tab=readme-ov-file)

To add the submodule you can use the following command. Remember to use the right destination folder too.

\$ git submodule add <https://github.com/psf/requests.git> external/requests

This should create the folder named ‘external’ in the try-submodules repository and add requests library in there as a submodule.

You can confirm this by checking out the .gitmodules file. Once the submodule library is in there, you can start using all the functionalities it provides.

<img src="/images/2024-01-31-using-git-submodules/image3.png" style="width:6.45833in;height:2.45833in" />

Try using the submodule with this example script in a file called script.py

<img src="/images/2024-01-31-using-git-submodules/image4.png" style="width:6.5in;height:3.39097in" />

As you can see, it’s able to fetch json data using the requests submodule.

There’s more information on using git submodules [here](https://git-scm.com/book/en/v2/Git-Tools-Submodules).
