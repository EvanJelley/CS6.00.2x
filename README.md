# CS 6.00.2x

This is a small repository of projects completed while taking an online offering of CS 6.00.2x on edX. This combined with the certificate that I have posted on my Trello board are intended to be proof of completion of the course and reasonable understanding of the material. The following subsections are brief summaries of the projects. Seeing as these projects are demonstrations of what was done in the course, I have elected to leave all of the comments and functions that were provided by the instructor. Typically students were provided a "bare bones" structure that they were asked to fill with the appropriate function/classes.

## Robot Simulation [(file)](Robot_Simulation/robotSim.py)

This is a simulation of robots cleaning a floor. The simulation allows the user to select the number of robots, size of floor, coverage, and a couple of different movement patterns. They then can test the efficiency of the different robot movements. The robotSimVisualize was provided by the instructor but is helpful for understanding how the different simulations will behave.

### Code Spotlight

I am especially proud of the following function definitions as they indicate my ability to incorporate class methods to implement succinct functions and incorporate calls to external py documents that I did not write.

```python
def runTrialWithoutAnimation(num_robots, speed, width, height, min_coverage, robot_type):
    timeSteps = 0
    room = RectangularRoom(width, height)
    robots = []
    for num in range(num_robots):
        robots.append(robot_type(room, speed))
    while True:
        if room.getNumCleanedTiles()/room.getNumTiles() >= min_coverage:
            return timeSteps
        else:
            for robot in robots:
                robot.updatePositionAndClean()
            timeSteps += 1

def runTrialWithAnimation(num_robots, speed, width, height, min_coverage, robot_type):
    anim = robotSimVisualize.RobotVisualization(num_robots, width, height)
    timeSteps = 0
    room = RectangularRoom(width, height)
    robots = []
    for num in range(num_robots):
        robots.append(robot_type(room, speed))
    while True:
        if room.getNumCleanedTiles()/room.getNumTiles() >= min_coverage:
            anim.done()
            return timeSteps
        else:
            anim.update(room, robots)
            for robot in robots:
                robot.updatePositionAndClean()
            timeSteps += 1
```
## Viral Simulation [(file)](Virus_Simulation/virusSimulation.py)

This simulation introduces a virus population and allows the user to see how variance in reproductivity probability, initial viral load, and resistance to medication effect virus population overtime. The final simulation primarily measures the introduction of medication on the percentage of the population that is antiviral resistant.

#### Learning:

A persistent bug in the development this program occurred when inoculating the patient with the viral load. I found that manipulation of the viruses in one patient would alter the same viruses in another patient. After a good deal of trouble shooting I finally discovered that my failure to make a copy of the list of Virus objects for each Patient was the culprit. So, a problem that took hours to solve only required only three symbols to fix: [:]

### Code Spotlight

This is the primary method of the patient class that incorporates the methods of the virus class to allow the patient to take a time-step and see how the virus population changes.

```python
def update(self):
    """
    Update the state of the virus population in this patient for a single
    time step. update() should execute these actions in order:

    - Determine whether each virus particle survives and update the list of
        virus particles accordingly

    - The current population density is calculated. This population density
        value is used until the next call to update().

    - Based on this value of population density, determine whether each 
        virus particle should reproduce and add offspring virus particles to 
        the list of viruses in this patient.
        The list of drugs being administered should be accounted for in the
        determination of whether each virus particle reproduces.

    returns: The total virus population at the end of the update (an
    integer)
    """
    # Handle clearing viruses that die
    clear_index = []
    for virus in self.viruses:
        index = 0
        if virus.doesClear():
            clear_index.append(index)
        index += 1
    for index in clear_index:
        self.viruses.pop(index)

    # Calculate pop density
    self.popDensity = len(self.viruses) / self.maxPop

    # Handle reproducing viruses
    new_viruses = []
    for v in self.viruses:
        try:
            newbie = v.reproduce(self.popDensity, self.activeDrugs)
            new_viruses.append(newbie)
        except NoChildException:
            continue
    for v in new_viruses:
        self.viruses.append(v)

    return self.getTotalPop()
```

## Climate Analysis [(file)](Climate_Analysis/climateAnalysis.py)

This was practice with taking in a fairly large dataset as using basic regression analysis to gain some insight. In this instance the data was daily temperature for 55 year time span. Of the three main projects completed in the course I found this to be the least compelling for the simple fact that I had already worked with regression analysis in my econometrics courses in university. 


### Code Spotlight

The capacity to write such a simple function to test a series of models is what makes Python so powerful. (Most of the code in this program is not very difficult to write, so I struggled to find an area that I wanted to highlight).

```python
def generate_models(x, y, degs):
    """
    Generate regression models by fitting a polynomial for each degree in degs
    to points (x, y).
    Args:
        x: a list with length N, representing the x-coords of N sample points
        y: a list with length N, representing the y-coords of N sample points
        degs: a list of degrees of the fitting polynomial
    Returns:
        a list of numpy arrays, where each array is a 1-d array of coefficients
        that minimizes the squared error of the fitting polynomial
    """
    models = []
    for i in degs:
        coefficients = np.polyfit(x, y, i)
        arr = np.array(coefficients)
        models.append(arr)
    return models

```

## Final Disclaimer

These small programs are by no means intended to be representative of user ready (nor fully functional) products, but instead simply evidence of my work in CS 6.00.2x. With a bit of work they could be expanded into larger models. However, I left them as learning tools and have moved on to other more involved projects that I will share as I develop them.