TLDR:
I created my first python package: `colab_host`. https://pypi.org/project/colab-host/
The idea is to be able to host any (python) application in colab/kaggle notebook environment.
GitHub: https://github.com/PuneethaPai/colab_host
Documentation: https://colab-host.readthedocs.io/en/latest/

Would appreciate if you can try out and share feedback on things working, not working, feature request, etc.

Long Version:
Google Colab and Kaggle notebooks are great. You have powerful compute, but just using their notebook environment feels restrictive.
Given the hardware you should be able to do more.

Now with colab_host you can bring up IDE of your choice for developing and host any application for testing purposes.
The package currently supports:
IDE:
    Jupyter Notebook
    Jupyter Lab
    For VSCode you can use colabcode package (https://pypi.org/project/colabcode/)
Applications:
    Flask, gunicorn application
    FastAPI, uvicorn application