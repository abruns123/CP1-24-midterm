### How to Run the Code

The unit converter_module has two functions which can convert feet to meters and yards to meters. The ft_to_m(ft) function takes in a value in feet and converts it to meters. The yd_to_m(yd) function takes in a value in yards and converts it to meters. They may be imported by the user such that the user may use them to convert whatever they need. 

The distance_calc module contains functions used to calculate the distance between two latitude/longitudinal points in units of meters. It does this using the assumption that the earth is a flat circle for simplicity. diffm(lat1,lat2,lon1,lon2) is the function the user should call to do this; the parameters it takes in are lat1, the initial latitudinal point, lat2, the new latitudinal point, lon1, the initial longitudinal point, and lon2, the new longitudinal point. It does this making the assumption that the earth is flat such that latitude is treated as the y-axis and longitude is treated as the x-axis. distance_calc also contains a module called reader that read out csv files such that the distance between many points may be calculated. It does this using a pandas function to read out the file and isolate columns containing latitude and longitude data. The user may call this function as reader(path) where path is the string containing the path name of the csv file containing longitudinal and latitudinal data labeled "Longitude (°)", Latitude (°). Two lists are output; the first list output contains latitude data, the second list output contains longitude data. Then, the user can use this data however they see fit to get distances between the points; I, for example used a loop: 
"for i in range(1,len(lats)-1):
    distances.append(dc.diffm(lats[i], lats[i+1], lons[i], lons[i+1]))"

to get a list called distances which contains the sequential distances I travel between each sequential moment in time. 

distance_calc also contains a recursive function get_coords(d,xy,i) which returns the x-y coordinates from a series of displacements. d is a list of displacements, xy is a displacement within that list of displacements, and i is the index of that displacement in the list. get_coords determines the xy-coordinates by summing all the displacements from the start of the list to the index i specified by the user. This function proves very useful when you want to plot x,y position.

The unix_time module contains functions used to calculate unix time. The user may call time(date) where date is a list of length 
6 as follows [year, month, day, hour, minute, second]. With this, time takes in this date information to return the unix time of that exact second. Of these values in the list, all but seconds must be integers; however, second may be a float or decimal value. unix_time also contains two functions for reading files, date_reader(path) and time_reader(path) which are called by get_unix_time(path1,path2). The user should import get_unix_time such that they may use their csv files to generate a list of unix time values from their data. path1 must be a csv file containing a column with time information labeled "time (s)", wheras path2 must be a csv file containing date information labeled "system time text".

direction_of_motion module contains functions designed to take in acceleration and time data and use that to calculate the direction of motion. The user should import the get_direction(times, acc), which is meant to take in two lists; times is a list of time values, acc is a list of acceleration values. get_direction will return a list of direction values that range from -1,0,1 based on the direction of motion indicated by the time and acceleration data. direction_of_motion also includes the acc_reader(path) function. The user should import this and set their input to a string which is the path to the file with data. This file should contain acceleration data labeled "Linear Acceleration z (m/s^2)" and time data labeled "Time (s)". What is returned is by acc_reader are two lists. The first is a list of the acceleration values, the second is a list of the time values. Effectively, acc_reader(path) gives you the lists you need to run get_direction(times, acc) with your data from a csv file. The primary flaw with this function is that it can only read the data so precisely such that it can't perfectly determine the direction of motion to such an extent that the motion at the end would be zero; to help this the make_end_zero(direction) function exists to detect a jostle in the elevator that indicates that it's stopped; if there is no jostle, it is assumed that the elevator has stopped at the end of the experiment, so the last value of direction is set to zero.
