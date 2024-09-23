# S2 Statistics for Data Science Coursework Submission (sd2022)

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## Description
This project is associated with the submission of the coursework for the S2 Statistics for Data Science Module as part of the MPhil in Data Intensive Science at the University of Cambridge. The Coursework Instructions can be found under [lighthouse.pdf](lighthouse.pdf). The associated project report can be found under [S2 Coursework Report](report/s2_sd2022_report.pdf).

## Table of Contents
- [Installation and Usage](#installation-and-usage)
- [Support](#support)
- [License](#license)
- [Project Status](#project-status)
- [Authors and Acknowledgment](#authors-and-acknowledgment)

## Installation and Usage

To get started with the code associated with the coursework submission, follow these steps:

### Requirements

- Python 3.9 or higher installed on your system.
- Conda installed (for managing the Python environment).
- Docker (if using containerisation for deployment).

### Steps

You can either run the code locally using a `conda` environment or with a container using Docker. The code is presented in the [main.ipynb](src/main.ipynb) Jupyter Notebook (located in the `sd2022/src` directory). The notebook will run faster locally on a high-spec computer (recommended). 

#### Local Setup (Using Conda) [RECOMMENDED]

1. **Clone the Repository:**

    Clone the repository to your local machine with the following command:

    ```
    $ git clone https://gitlab.developers.cam.ac.uk/phy/data-intensive-science-mphil/S2_Assessment/sd2022
    ```

    or simply download it from [S2 Statistics for Data Science Coursework (sd2022)](https://gitlab.developers.cam.ac.uk/phy/data-intensive-science-mphil/S2_Assessment/sd2022).

2. **Navigate to the Project Directory:**

    On your local machine, navigate to the project directory with the following command:

    ```
    $ cd /full/path/to/sd2022
    ```

    and replace `/full/path/to/` with the directory on your local machine where the repository lives in.

3. **Setting up the Environment:**

    Set up and activate the `conda` environment with the following command:

    ```
    $ conda env create -f environment.yml
    $ conda activate s2_sd2022_env
    ```

4. **Install ipykernel:**

    To run the notebook cells with `s2_sd2022_env`, install the ipykernel package with the following command:

    ```
    python -m ipykernel install --user --name s2_sd2022_env --display-name "Python (s2_sd2022_env)"
    ```


4. **Open and Run the Notebook:**

    Open the `sd2022` directory with an integrated development environment (IDE), e.g. VSCode or PyCharm, select the kernel associated with the `s2_sd2022_env` environment and run the [main.ipynb](src/main.ipynb) Jupyter Notebook (located in the `sd2022/src` directory).


#### Containerised Setup (Using Docker)

1. **Clone the Repository:**

    Clone the repository to your local machine with the following command:

    ```
    $ git clone https://gitlab.developers.cam.ac.uk/phy/data-intensive-science-mphil/S2_Assessment/sd2022
    ```

    or simply download it from [S2 Statistics for Data Science Coursework (sd2022)](https://gitlab.developers.cam.ac.uk/phy/data-intensive-science-mphil/S2_Assessment/sd2022).

2. **Navigate to the Project Directory:**

    On your local machine, navigate to the project directory with the following command:

    ```
    $ cd /full/path/to/sd2022
    ```

    and replace `/full/path/to/` with the directory on your local machine where the repository lives in.

3. **Install and Run Docker:**

    You can install Docker from the official webpage under [Docker Download](https://www.docker.com/).
    Once installed, make sure to run the Docker application.

4. **Build the Docker Image:**

    You can build a Docker image with the following command:

    ```
    $ docker build -t [image] .
    ```

    and replace `[image]` with the name of the image you want to build.

4. **Run a Container from the Image:**

    Once the image is built, you can run a container based on this image:

    ```
    $ docker run -p 8888:8888 [image]
    ```

    This command starts a container from the `[image]` image and maps port `8888` of the container to port `8888` on your local machine. The Jupyter Notebook server within the container will be accessible on JupyterLab at [http://localhost:8888](http://localhost:8888). 

5. **Access and Run the Notebook:**

    After running the container, you'll see logs in the terminal containing a URL with a token. It will look similar to this:

    ```
     http://127.0.0.1:8888/lab?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    ```
    
    Navigate to [http://localhost:8888](http://localhost:8888) and enter the token `XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`. Once you accessed JupyterLab, run the [main.ipynb](src/main.ipynb) Jupyter Notebook (located in the `sd2022/src` directory) with an `ipykernel` (Python 3).

    **Note:** Make sure that no other Jupter Notebook Servers are running. Otherwise, you might encounter 'Invalid credentials' issues when entering the token. Close any running Jupter Notebook Servers. To stop a running server, use `Ctrl + C` in the terminal where you launched JupyterLab. Also make sure port `8888` is not occupied.


## Support
For any questions, feedback, or assistance, please feel free to reach out via email at [sd2022@cam.ac.uk](sd2022@cam.ac.uk).

## License
This project is licensed under the [MIT License](https://opensource.org/license/mit/) - see the [LICENSE](LICENSE) file for details.

## Project Status
The project is in a state ready for submission. All essential features have been implemented, and the codebase is stable. Future updates may focus on minor improvements, bug fixes, or optimisations.

## Use of auto-generation tools
GitHub Co-Pilot assisted the author in producing all function docstrings present in the project repository. No specific commands have been given, instead auto-completion suggestions have occasionally been accepted. Other uses of auto-generation tools:

#### ChatGPT Prompt 1: What is a fast way of tracking the running mean and standard deviation in Python?

- ChatGPT output: 

```python
def update_running_stats(count, mean, M2, new_value):
    count += 1
    delta = new_value - mean
    mean += delta / count
    delta2 = new_value - mean
    M2 += delta * delta2
    return count, mean, M2

def finalize_stats(count, M2):
    variance = M2 / count if count > 1 else float('nan')
    stddev = variance ** 0.5
    return variance, stddev
```
- Modification of the output:

```python
def welfords_method(samples):
    """
    Calculate the running mean and standard deviation of a set of samples using
    Welford's method.

    References
    ----------
    Welford, B.P. (1962) Note on a method for calculating corrected
    sums of squares and products. Technometrics, 4(3), 419-420.
    """
    # Calculate running mean
    running_mean = np.cumsum(samples)/(np.arange(len(samples)) + 1) 
    # Initialise variables
    n = 0
    mean = 0
    M2 = 0
    running_std = np.zeros(len(samples))
    # Loop through the samples
    for i, x in enumerate(samples):
        n += 1
        # Calculate mean and M2
        delta = x - mean
        mean += delta / n
        delta2 = x - mean
        M2 += delta * delta2
        # Calculate running standard deviation
        if n < 2:
            running_std[i] = float('nan')
        else:
            variance = M2 / (n - 1)
            running_std[i] = np.sqrt(variance)
    return running_mean, running_std
```

## Authors and Acknowledgment
This project is maintained by [Steven Dillmann](https://www.linkedin.com/in/stevendillmann/) at the University of Cambridge.

28th March 2024
