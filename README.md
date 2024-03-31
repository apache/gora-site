# Apache Gora Website

Welcome to the Apache Gora website repository! This README provides guidance on building the website locally for development or testing purposes.

## Building Locally

To build this website locally, follow these steps:

### Prerequisites
- Ensure that you have [Python 3](https://python.org) installed on your system.

### Setup
1. Clone this repository to your local machine.

```bash
git clone https://github.com/apache/gora-site.git
```

2. (Optional) Set up a new Python virtual environment.

```bash
python3 -m venv env
source env/bin/activate
```

3. Install the required Python packages using pip.

```bash
pip install -r requirements.txt
```
> Note: You may also need to run `pip install "pelican[markdown]"`

### Building
4. Utilize Pelican to generate the website.

```bash
pelican content
```

5. Once Pelican has completed generating the website, you can locate the output in the `output` directory.

## Contributing
If you encounter any issues with the website or have suggestions for improvements, please feel free to [open an issue](https://github.com/apache/gora-site/issues) or submit a pull request. We welcome contributions from the community!
