'''
+=======================================================================================+
|   	            Traveling Salesman (Hillclimber heuristic search)	                |
|   			                 Alyssa Herbst                			|
|   				        January 20 2015 			        |
+=======================================================================================+
'''
##############################################<START OF PROGRAM>##############################################
def setUpCanvas(root): # These are the REQUIRED magic lines to enter graphics mode.
    root.title("THE TRAVELING SALESMAN PROBLEM by (your name here).")
    canvas = Canvas(root, width  = root.winfo_screenwidth(), height = root.winfo_screenheight(), bg = 'black')
    canvas.pack(expand = YES, fill = BOTH)
    return canvas
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def script(x, y, msg = '', kolor = 'WHITE'):
    canvas.create_text(x, y, text = msg, fill = kolor,  font = ('Helvetica', 10, 'bold'))
#---------------------------------------------------------------------------------Traveling Salesman Problem-

def plot(city): # Plots 5x5 "points" on video screen
    x = city[1]+5; y = city[2]+5 # The +5 is to push away from the side bars.
    if city[0] == -1:
        kolor = 'WHITE'
    else: kolor = 'YELLOW'
    canvas.create_rectangle(x-2, y-2, x+2, y+2, width = 1, fill = kolor)
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def line(city1, city2, kolor = 'RED'):
    canvas.create_line(city1[1]+5, city1[2]+5, city2[1]+5, city2[2]+5, width = 1, fill = kolor)
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def displayDataInConsole(AlgorithmResults, baseCity, city):
    print('===<Traveling Salesman Path Statistics>===')
    print ('   algorithm         path length ')
    for element in AlgorithmResults:
           print ('---%-20s'%element[0], int(element[2]))
    city.sort()
    baseCity.sort()
    if city == baseCity:
        print("---Data verified as unchanged.")
    else:
        print ('ERROR: City data has been corrupted!')
    print('   Run time =', round(clock()-START_TIME, 2), ' seconds.')
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def printCity(city): # Used for debugging.
    count = 0
    for (id,x,y) in city:
        print( '%3d: id =%2d, (%5d, %5d)'%(count,id, int(x), int(y)))
        count += 1
#---------------------------------------------------------------------------------Traveling Salesman Problem--
def displayElapsedTime (msg = 'time'):
    length = 30
    msg = msg [:length]
    tab = '.'*(length-len(msg)) # <-- msg length truncated at 30 chars
    print('--' + msg.upper() + tab + ' ', end = '')
    time = round(clock() - START, 1) # START is global constant.
    print( chr(9200) + '%2d'%int(time/60), ' min :', '%4.1f'%round(time%60, 1), \
           ' sec', sep = '') 

def displayPathOnScreen(city, statistics):
#=---Normalize data
    (minX, maxX, minY, maxY, meanX, meanY, medianX, medianY, size, m, b) = statistics
    canvas.delete('all')
    cityNorm, (p,q,r,s) = normalizeCityDataToFitScreen(city[:], statistics)

#---Plot points and lines
    cityNorm.append(cityNorm[0])
    plot(cityNorm[0])
    for n in range(1, len(cityNorm)):
        plot(cityNorm[n])
        line(cityNorm[n], cityNorm[n-1])
    script(650,  20, 'path length = ' + str(pathLength(city)))
    canvas.create_rectangle(530,10, 770, 30, width = 1, outline = 'WHITE')
    canvas.update()
    root.mainloop() # Required for graphics.
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def normalizeCityDataToFitScreen(city, statistics):
    """ Coordinates are all assumed to be non-negative."""
    (minX, maxX, minY, maxY, meanX, meanY, medianX, medianY, size, m, b) = statistics
    newCity = []

#---Step 1a. Shift city points to the x- and y-axes.
    for (id, x,y) in city:
        newCity.append ( (id, x-minX, y-minY))

#---Step 1b. Shift line-of-best-fit to the x- and y-axes.
    (x0,y0) = (maxX-minX, m*maxX+b - minY) # = x-intercept of line-of-best-fit.
    (x1,y1) = (minX-minX, m*minX+b - minY) # = y-intercept of line-of-best-fit.


#---Step 1c. Shift max-values to x- and y-axes.
    maxX = maxX-minX
    maxY = maxY-minY

#---Step 2a. # Re-scale city points to fit the screen.
    cityNorm = []
    for (id, x, y) in newCity:
        cityNorm.append ((id, x*SCREEN_WIDTH/maxX, y*SCREEN_HEIGHT/maxY))

#---Step 2b. # Re-scale the x-axis and y-axis intercepts for the line-of-best-fit.
    (x0,y0) = x0/maxX*SCREEN_WIDTH, y0/maxY*SCREEN_HEIGHT # a point on the x-axis
    (x1,y1) = x1/maxX*SCREEN_WIDTH, y1/maxY*SCREEN_HEIGHT # a point on the y-axis

    return cityNorm, (x1,y1,x0,y0) # = the adjusted city xy-values and 2 points on the line-of-best-fit.
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def readDataFromFileAndAppendId(fileName):
    file1 = open(fileName, 'r')
    fileLength = int(file1.readline()) # removes heading
    city = []
    for elt in range(fileLength):
       x, y = file1.readline().split()
       city.append( [0, float(x), float(y)] ) # A place for an id (0, here) is appended.
    file1.close()
    return city
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def pathLength(city):
    totalPath = 0
    for n in range(1, len(city)):
        totalPath += dist( city[n-1], city[n] )
    totalPath += dist( city[n], city[0] )
    return int(totalPath)
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def dataStatistics(city):
    xValues = []
    yValues = []
    size = len(city)
    for (id, x,y) in city:
        xValues.append(x)
        yValues.append(y)
    minX = min(xValues)
    maxX = max(xValues)
    minY = min(yValues)
    maxY = max(yValues)

    assert (minX >= 0 or maxX >= 0 or minY >= 0 or maxY >= 0)

    meanX = sum(xValues)/size
    meanY = sum(yValues)/size
    medianX = city[len(city)//2][0]
    medianY = city[len(city)//2][1]

#---Derive the line of best fit: y = mx+b
    xyDiff   = 0
    xDiffSqr = 0
    for (id, x,y) in city:
        xyDiff  += (meanX - x)*(meanY - y)
        xDiffSqr+= (meanX - x)**2

    m = xyDiff/xDiffSqr
    b = meanY - m*meanX

    return minX, maxX, minY, maxY, meanX, meanY, medianX, medianY, size, m, b
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def dist(cityA, cityB):
    return hypot(cityA[1]-cityB[1], cityA[2] - cityB[2])
#---------------------------------------------------------------------------------Traveling Salesman Problem--

def sortY(city):#city[a][2]
    city.sort(key = itemgetter(2))
    return city

def sortX(city):
    city.sort(key = itemgetter(1))
    return city

def theta(cityA, cityB, cityC):#returns in degrees, for easy debugging
    ABx = cityB[1] - cityA[1]
    BCx = cityC[1] - cityB[1]
    ABy = cityB[2] - cityA[2]
    BCy = cityC[2] - cityB[2]
    magAB, magBC = dist(cityA, cityB), dist(cityB, cityC)
    return ((acos((ABx*BCx + ABy*BCy)/(magAB * magBC)))*180 / pi) #uses dot product to find theta

def swap(indexA, indexB, city):
    temp = city[indexB]
    city[indexB] = city[indexA]
    city[indexA] = temp

def cross(cityA, cityB, cityC, cityD):
    t1 = theta(cityA, cityC, cityB)
    t2 = theta(cityD, cityC, cityB)
    t3 = theta(cityC, cityB, cityD)
    t4 = theta(cityC, cityB, cityA)
    print(t1, t2, t3, t4)
    return (t1 > t2) and (t3 > t4)

def sortHillClimbing(city, trials):#dist(a, b)

    length = len(city) 
    for x in range(trials):#best dist
        bestDist = float('inf')
        bestDifference = float('inf')
        i1 = int(random()* length)
        bestI1 = i1
        bestI2 = i1
        for i2 in range(length):
            dist1 = dist(city[((i1 - 1 < 0)*(length - 1)) or (i1 - 1)], city[i1]) + \
                dist(city[i1], city[(i1 + 1)%length]) + \
                dist(city[((i2 - 1 < 0)*(length - 1)) or (i2 - 1)], city[i2]) + \
                dist(city[i2], city[(i2 + 1)%length])

            dist2 = dist(city[((i1 - 1 < 0)*(length - 1)) or (i1 - 1)], city[i2]) + \
                dist(city[i2], city[(i1 + 1)%length]) + \
                dist(city[((i2 - 1 < 0)*(length - 1)) or (i2 - 1)], city[i1]) + \
                dist(city[i1], city[(i2 + 1)%length])

            difference = dist2 - dist1
            
            if difference < 0 and difference < bestDifference:
                bestDifference = difference
                bestI1 = i1
                bestI2 = i2     
        swap(bestI1, bestI2, city)#swap the 2 that will make the greatest distance
    return city
    
def mySort(city):#takes lists of hypots, smallest hypot, next in list.  shorten city list.
    cityA = city[0]
    cityB = city[1]
    cityC = city[2]
    cityD = city[3]
    print('Do cities', cityA, cityB, cityC, cityD, 'cross:  ', cross(cityA, cityB, cityC, cityD))
    #print('theta between', cityB, cityA, cityC, ' = ', theta(cityA, cityB, cityC))
    
    #####STOPPED HERE
    #Not done testing theta function.  They cross if t(acb)> t(dcb) and T(cbd) > t(cba)
    

#====================================<GLOBAL CONSTANTS and GLOBAL IMPORTS>========Traveling Salesman Problem==

from tkinter   import Tk, Canvas, YES, BOTH
from operator  import itemgetter
from itertools import permutations
from copy import deepcopy
from random    import shuffle, random
from time      import clock
from math      import hypot, acos, asin, pi
root           = Tk()
canvas         = setUpCanvas(root)
START     = clock()
SCREEN_WIDTH   = root.winfo_screenwidth() //5*5 - 15 # adjusted to exclude task bars on my PC.
SCREEN_HEIGHT  = root.winfo_screenheight()//5*5 - 90 # adjusted to exclude task bars on my PC.
RANDOMIZEDTRIALS = 1000
fileName       = "tsp.txt" # My file name will be different from yours
#==================================================< MAIN >=======================Traveling Salesman Problem==

def main():
#---0. Read in data, append an id to every pair, and store results in a variable called "city".
    city  = readDataFromFileAndAppendId(fileName)
    #city = [[id, 0, 0], [id, 1, 0], [id, 0, 1],[id, 1, 1]]

#---1. Extract statistics.
    statistics = (minX, maxX, minY, maxY, meanX, meanY, medianX, medianY, size, m, b) = dataStatistics(city)

#---2. Create a random path.
    shuffle(city)
    
#---3. Sort on y-coordinate and connect sequentially by y.
    #city = sortY(city)
#---4. Sort on x-coordinate and connect sequentially by x.
    #city = sortX(city)
    city = sortHillClimbing(city, RANDOMIZEDTRIALS)
    #print(city)
#---5. Your algorithm(s). Can you do better than the sorting algorithms above?
    #mySort(city)
#---6. Display results.
    displayPathOnScreen(city, statistics)
    displayElapsedTime('end of program')
#---------------------------------------------------------------------------------Traveling Salesman Problem--
if __name__ == '__main__': main()
###############################################<END OF PROGRAM>###############################################
