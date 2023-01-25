FROM python

WORKDIR /app

COPY . /app/

CMD ["python", "rng.py"]

# If we use docker run it will not allow for the inputs in the python file to work
# Looking through docker run --help there are two flags that will help
# docker run -i - which is the interactive mode allopwing for inputs
# docker run -t - which opens a psudo terminal
# we casn combine the two with docker run -it <id>|<name>
