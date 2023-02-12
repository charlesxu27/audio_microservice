# Microservice Description

## Background

This microservice connects to a TUI to eneable users to predict noise levels from sound sources to specified receiver locations. Users can add entries to a library of “noise generators” by either octave-band or overall dBA. Users can then add a “source” to a location on the “sound map” and assign it one of the “sound generators” from the library. Once a “receiver” is added to the “sound map”, the user will be able to view a 2D representation of the “sound map” which shows relative distances between the elements and the predicted levels at the receivers. 

## Usage

This microservice employs RPyC3, a python library faciliating remote prodedure calls, clustering, and distributed-computing. It allows the user to execute python code in a different process or machine altogether. Here, we have a client which sends data to the server. The server generates a 2D string representation of a sound map for the main TUI.

### Requesting Data

1. The client connects to the server with the following lines:

```python
import rpyc
conn = rpyc.connect("localhost", 18861) 
```

2. After connecting to the server, the client can access exposed functions to send a request and receive a response. For example, the server has a method called `def exposed_get_grid(self, max_print_dimension, data_1, data_2) -> str:`. The client can call on this exposed function with the following lines:

```python
# define the data
data_1 = ("s", ["01", "02"], [(1.6, 33.2), (82.6, 41.2)], "\033[93m")
data_2 = ("r", ["03", "04"], [(353.2, 112.5), (523.9, 5.3)], "\u001b[34m")

result = conn.root.get_grid(10, data_1, data_2)
```

### Receiving Data

3. After receiving the data and storing the response in `result`, the client can integrate the data into another program. In this case, we will print the response and close the connection:

```python
print(result)
conn.close()
```

## System Design

![Screen Shot 2023-02-12 at 13 03 44](https://user-images.githubusercontent.com/77357320/218336987-254fa656-08b0-453b-a58c-4efaa0c6917d.png)
![Screen Shot 2023-02-12 at 13 03 52](https://user-images.githubusercontent.com/77357320/218336997-bb27374d-5c16-4373-842d-56a2f2afe6e8.png)
