Interactive quantum programming with Forest & Jupyter
=====================================================

The `forest-notebook` repository contains the [`Dockerfile`][dockerfile]
for building the [`rigetti/forest-notebook`][forest-notebook] image.
This image contains an interactive quantum programming environment
backed by [JupyterLab][jupyter], akin to the applications available
in the [Docker Stacks][docker-stacks] repository.

The image is based off of the [`rigetti/forest`][forest] image, which comes
with [pyQuil][pyquil] installed, as well as [quilc][quilc] and [QVM][qvm]
servers running in the background. The `rigetti/forest-notebook` image
additionally has the [`forest-benchmarking`][benchmarking] library installed,
along with some useful Python packages for data analysis and visualization.
Finally, it contains `jupyter` and the new JupyterLab interface, and is
configured to spin up a notebook server when the image is run, which can
be done via the following command (replacing `PORT` with the `localhost`
port you'd like to run the notebook server on):

```bash
docker run -p PORT:8888 rigetti/forest-notebook
```

This will start the container, and somewhere in the terminal output it will
print a URL that looks something like the following, but with `TOKEN` replaced
with a long string of letters and numbers:

```
http://127.0.0.1:8888/?token=TOKEN
```

Copy paste the above URL into your browser, replacing 8888 with `PORT`. This
will bring up the JupyterLab interface, with the root directory of the
filesystem being the top-level directory of the pyQuil repository. You can
then run the notebooks in the `examples` directory, or create your own.
These notebooks will have access to the latest quilc and QVM servers,
which are running in the background. Happy quantum programming!

Creating a Binder repository using this image
---------------------------------------------

One of the most exciting recent applications of a Jupyter-backed Docker
image is [Binder][binder], which provides a free hosting service and
executable environment for a repository of Jupyter notebooks. Binder can
be configured many ways, but the most advanced configuration is via a custom
Dockerfile. To create a Binder that has access to the full Forest quantum
programming suite, add the following Dockerfile to the repository of Jupyter
notebooks that you'd like to turn into an interactive web application:

```dockerfile
# build image from a tagged forest-notebook image
FROM rigetti/forest-notebook:2.16.0

# copy over files from binder repository into $HOME
COPY . ${HOME}

# set working directory to $HOME
WORKDIR ${HOME}
```

Once you've added the Dockerfile, follow the instructions in the Binder link
above to create your very own interactive quantum programming environment!

[benchmarking]: https://github.com/rigetti/forest-benchmarking
[binder]: https://mybinder.org
[dockerfile]: https://docs.docker.com/engine/reference/builder/
[docker-stacks]: https://github.com/jupyter/docker-stacks
[forest]: https://hub.docker.com/r/rigetti/forest
[forest-notebook]: https://hub.docker.com/r/rigetti/forest-notebook
[jupyter]: https://jupyter.org/
[pyquil]: https://github.com/rigetti/pyquil
[quilc]: https://github.com/rigetti/quilc
[qvm]: https://github.com/rigetti/qvm
