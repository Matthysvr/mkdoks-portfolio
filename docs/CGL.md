# CGL
## Conway's Game of Life


``` py title="Load Packages"
import random
import numpy as np
from scipy.signal import convolve2d
```

``` py title="Generate initial random matrix"
def create_random_matrix(rows, cols, fraction, seed=None):
    # Set the random seed if provided
    if seed is not None:
        random.seed(str(seed))
    else:
        seed = str(random.randint(0,2000000))
        print("Seed: " + seed)
        random.seed(seed)

    # Calculate the total number of elements in the matrix
    total_elements = rows * cols

    # Calculate the number of 1's based on the fraction
    num_ones = int(total_elements * fraction)

    # Create a matrix of zeros
    matrix = np.zeros((rows, cols))

    # Set random positions to 1
    positions = random.sample(range(total_elements), num_ones)
    for pos in positions:
        row = pos // cols
        col = pos % cols
        matrix[row, col] = 1

    return matrix
```

``` py title="Action CGL steps using convolution"
def game_of_life(matrix, steps):
    kernel = np.array([[1, 1, 1],
                       [1, 0, 1],
                       [1, 1, 1]])

    for _ in range(steps):
        # Perform convolution with the kernel
        neighbors_count = convolve2d(matrix, kernel, mode='same', boundary='wrap')

        # Apply the rules of the Game of Life
        matrix = np.logical_or(np.logical_and(matrix, neighbors_count == 2),
                               neighbors_count == 3)

    return matrix
```

``` py title="Print the matrix"
def render_matrix(matrix):
    for row in matrix:
        for cell in row:
            if cell:
                print("█", end="")
            else:
                print(" ", end="")
        print()
```

``` py title="Example"
render_matrix(game_of_life(create_random_matrix(25,150,0.1005,554874),1))
```

```
                                                      █                                           █      █                                           █
                                                      ███              ██                               █                                    █ █      
                                                              ██       ███                            █                                        █      
                                                              ███      ███                             █                          █                   
                 █          ██                                  ██ █   █                                                                              
               ███          █                                      █         █   ██                                                                   
               ██ █                                                █         ███                               █                                      
             █ █                              █                                                          ██                                           
  ███                                  █                          █                               █                      █ █                          
  ███                                                    █        █                               ██                ██        █ █          ██         
  ███          ██                                                              █                    █              ███          ██         ██         
                                                                              ████    █           ███                           █ █        █          
                                                                                ██    ██          █                                                   
                                                                                      █           █                                                   
                                       ██                                                                                                             
█ █                                    ██                                         █                                    █               █ █       █    
  █                                                         █                                                         ██                          ██  
  █         ████                           ██                                                                         █        █                 █    
               ██            █              █      █                                                                                           ███    
              ███            █      ███                                                                                                        ██     
    ██    ██   █                    ███                        █                                                                           █          
                █                    █                         █                                                                           █          
                                                     █       █ █ █        █                                                                           
                                                    ███          █        █          ███         █     █                                              
                                                     █                   ██                       █    ██                                            █
```