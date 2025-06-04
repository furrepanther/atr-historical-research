## Running OCR software in a virtual environment

### Why are virtual environments recommended?

A virtual environment is a self-contained setup on a server or local machine that allows you to install specific versions of software, such as Python libraries, without affecting your system-wide
setup. This helps you run your chosen OCR software smoothly without causing dependency conflicts for other programmes. OCR tools, in particular, require several dependencies that can conflict with other projects.

### Setting up a virtual environment

Depending on your operating system, you should be able to find many instructions and tutorial videos on how to create a virtual environment. In general, you first need to activate it, then you install required
software packages. On Linux and Mac, you can use the following commands:

```
# Create
python3 -m venv myenv

# Activate
source myenv/bin/activate

# Install packages (e.g., Kraken OCR)
pip install kraken

# Deactivate
deactivate
```

The commands to use on Windows are:

```
# Create
python -m venv myenv

# Activate
myenv\Scripts\activate

# Install packages
pip install kraken

# Deactivate
deactivate
```

> [!IMPORTANT]
> It is recommended to keep each project (e.g. one OCR model) in its own environment.
