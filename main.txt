# First Name: Jeff
# Last Name: Fenwick
# Student ID: 001549011

# Update mileage on trucks

import csv

# Hash table class

location_size = 27
visited_list = []


class ChainingHashTable:
    # Hash table constructor with capacity for 40 packages
    def __init__(self, initial_capacity=40):
        self.table = []
        for i in range(initial_capacity):
            self.table.append([])

    # Hash table insert as well as update function
    def insert(self, key, item):
        bucket = hash(key) % len(self.table)
        bucket_list = self.table[bucket]

        for kv in bucket_list:
            if kv[0] == key:
                kv[1] = item
                return True

        key_value = [key, item]
        bucket_list.append(key_value)
        return True

    def update_package_status(self, pnumb, status):
        temp = str(myHashPackage.search(pnumb))
        temp_str = temp.split(', ')

        package = Package(temp_str[0], temp_str[1], temp_str[2], temp_str[3], temp_str[4], temp_str[5],
                          temp_str[6], status)

        myHashPackage.insert(pnumb, package)

    # Remove item from hash table
    def remove(self, key):
        bucket = hash(key) % len(self.table)
        bucket_list = self.table[bucket]

        for kv in bucket_list:
            if kv[0] == key:
                bucket_list.remove(kv[0], kv[1])

    # Input package number and return full string of package information
    def search(self, key):
        bucket = hash(key) % len(self.table)
        bucket_list = self.table[bucket]

        for kv in bucket_list:
            if kv[0] == key:
                return kv[1]
            return None

    # Lookup function with separate UI that allows users to look up a package using different criteria
    def lookup(self):
        print('\nHow would you like to look up package?')
        inp = int(input(
            '\n0) Exit: \n1) Number: \n2) Street Address: \n3) Deadline: \n4) City: \n5) Zip code: \n6) Weight: \n'
            '7) Status: \n'))
        while inp != 0:
            if inp == 1:
                numbInp = int(input('\nPackage number?: '))
                print(myHashPackage.search(numbInp))
                return
            if inp == 2:
                streetInp = input('\nStreet address?: ')
                for i in range(len(myHashPackage.table)):
                    streetStr = str(myHashPackage.search(i + 1))
                    splitStr = streetStr.split(', ')
                    if streetInp in splitStr[1]:
                        print(streetStr)
                return
            if inp == 3:
                deadline_inp = input('\nDeadline?: ')
                for i in range(len(myHashPackage.table)):
                    deadline_str = str(myHashPackage.search(i + 1))
                    splitStr = deadline_str.split(', ')
                    if deadline_inp in splitStr[5]:
                        print(deadline_str)
                return
            if inp == 4:
                city_inp = input('\nCity?: ')
                for i in range(len(myHashPackage.table)):
                    city_str = str(myHashPackage.search(i + 1))
                    splitStr = city_str.split(', ')
                    if city_inp in splitStr[2]:
                        print(city_str)
                return
            if inp == 5:
                zip_inp = input('\nZip code?: ')
                for i in range(len(myHashPackage.table)):
                    zip_str = str(myHashPackage.search(i + 1))
                    splitStr = zip_str.split(', ')
                    if zip_inp in splitStr[4]:
                        print(zip_str)
                return
            if inp == 6:
                weight_inp = input('\nWeight(kg)?: ')
                for i in range(len(myHashPackage.table)):
                    weight_str = str(myHashPackage.search(i + 1))
                    splitStr = weight_str.split(', ')
                    if weight_inp in splitStr[6]:
                        print(weight_str)
                return
            if inp == 7:
                status_inp = input('\nStatus(AT_HUB, EN_ROUTE, DELIVERED?: ')
                for i in range(len(myHashPackage.table)):
                    status_str = str(myHashPackage.search(i + 1))
                    splitStr = status_str.split(', ')
                    if status_inp in splitStr[7]:
                        if status_str == 'DELIVERED':
                            print('Time Delivered: {}'.format(status_inp))  # Fill in with time delivered
                        print(status_str)
                return
        return

        # Reverse search function for determining the location number based on the address for inputting coordinates into

    # distance dictionary
    def rev_search(self, address_inp):
        for i in range(location_size):
            address_search = str(myHashAddresses.search(i))
            ret_numb = myHashAddresses.table[i]
            # Needed because some address data is slightly different between package data and distance data
            if address_inp[0:7] in address_search:
                return int(ret_numb[0][0])

    # Input package number and get back location number
    def loc_from_pnumb(self, pnumb):
        street_str = str(myHashPackage.search(pnumb))
        split_str = street_str.split(', ')
        return myHashAddresses.rev_search(split_str[1])

    def pnumb_from_loc(self, loc):
        temp_list = []
        for i in range(len(myHashPackage.table)):
            temp_address = str(myHashPackage.search(i + 1))
            str_address = temp_address.split(', ')
            if loc == myHashAddresses.rev_search(str_address[1]):
                temp_list.append(int(str_address[0]))
        return temp_list

    def add_from_pnumb(self, pnumb):
        street_str = str(myHashPackage.search(pnumb))
        split_str = street_str.split(', ')
        return split_str[1]


# Defines the package type of object
class Package:
    def __init__(self, pnumber, street_address, city, state, zipcode, deadline, weight, status):
        self.pnumber = pnumber
        self.street_address = street_address
        self.city = city
        self.state = state
        self.zipcode = zipcode
        self.deadline = deadline
        self.weight = weight
        self.status = status

    # Adds strings so that hash table data is returned instead of storage locations
    def __str__(self):
        return "%s, %s, %s, %s, %s, %s, %s, %s" % (self.pnumber, self.street_address, self.city, self.state,
                                                   self.zipcode, self.deadline, self.weight, self.status)


# Loads package data and inserts into table myHashPackage
def load_package_data(filename):
    with open(filename) as WGUPSPackageFile:
        package_data = csv.reader(WGUPSPackageFile, delimiter=',')
        for i in range(8):
            next(package_data)
        for package in package_data:
            pID = int(package[0])
            pstreet_address = package[1]
            pcity = package[2]
            pstate = package[3]
            pzipcode = package[4]
            pdeadline = package[5]
            pweight = package[6]
            pstatus = 'AT_HUB'

            package = Package(pID, pstreet_address, pcity, pstate, pzipcode, pdeadline, pweight, pstatus)

            myHashPackage.insert(pID, package)


# Loads distance data from file and inserts into dictionary with 2 dimensional keys
def load_distance_data(filename):
    with open(filename) as WGUPSDistanceTable:
        distance_data = csv.reader(WGUPSDistanceTable, delimiter=',')
        # Needed to advance the row count to start at the data portion
        for i in range(8):
            next(distance_data)
        count = 0
        for row in distance_data:
            for i in range(27):
                if row[i + 2] == '':
                    continue
                else:
                    myDistanceData[i, count] = float(row[i + 2])
            count += 1


# Populates a hash table with just addresses
def load_address_data(filename):
    with open(filename) as WGUPSPackageFile:
        address_data = csv.reader(WGUPSPackageFile, delimiter=',')
        j = 0
        for i in range(8):
            next(address_data)
        for package in address_data:
            l_address = package[0]
            location = Address(l_address)
            myHashAddresses.insert(j, location)
            j += 1


# Creates the address class so that just address data can be added to a hash table
class Address:
    def __init__(self, loc_address):
        self.loc_address = loc_address

    def __str__(self):
        return "%s" % self.loc_address


# Returns a distance given two location numbers
def dist_locations(inp_loc1, inp_loc2):
    # Only half of the distance table is populated to avoid redundancy, this ensures that only half can be selected
    if inp_loc1 > inp_loc2:
        inp_temp = inp_loc1
        inp_loc1 = inp_loc2
        inp_loc2 = inp_temp
    return round(float(myDistanceData[(inp_loc1, inp_loc2)]), 1)


def closest_location(c_loc, c_loc_visited_list):
    loc_number = 0
    loc = int(c_loc)
    low_dist = 100.0
    x = loc
    y = loc
    while x > 0:
        x -= 1
        if x in c_loc_visited_list:
            continue
        if dist_locations(x, y) < low_dist:
            low_dist = dist_locations(x, y)
            loc_number = x
    x = loc
    while y < location_size - 1:
        y += 1
        if y in c_loc_visited_list:
            continue
        if dist_locations(x, y) < low_dist:
            low_dist = dist_locations(x, y)
            loc_number = y
    return '{}, {}'.format(low_dist, loc_number)


def package_update():
    # Loads truck 3, adds route location, changes status of packages, and adds distance after truck 2 returns to hub
    for key, value in truck_list[1].route.items():
        if key == 0 and total_miles_traveled() > value:
            for x in truck3_packages:
                truck_list[2].load_truck(x)
                myHashPackage.update_package_status(x, 'EN_ROUTE / TRUCK_3')
            for x in truck3_route:
                truck_list[2].add_route_locations(x)
            cur_loc = 0
            for keys in truck_list[2].route:
                truck_list[2].route[keys] = dist_locations(cur_loc, keys) + truck_list[1].route[0]
                cur_loc = keys
    # Reloads truck 1 with the second set of packages as well as updating the status
    for key, value in truck_list[0].route.items():
        if key == 0:
            if total_miles_traveled() >= value:
                for x in truck1_reload_packages:
                    truck_list[0].load_truck(x)
                    myHashPackage.update_package_status(x, 'EN_ROUTE / TRUCK_1')
    # Changes status for and removes packages that have been delivered by truck 1
    for key, value in truck_list[0].route.items():
        if total_miles_traveled() >= value:
            for x in myHashPackage.pnumb_from_loc(key):
                if x in truck_list[0].packages:
                    myHashPackage.update_package_status(x, 'Delivered at {} by Truck 1'
                                                        .format(clock.show_time_delivered(value)))
                    truck_list[0].packages.remove(x)
    # Changes status for and removes packages that have been delivered by truck 2
    for key, value in truck_list[1].route.items():
        if total_miles_traveled() >= value:
            for x in myHashPackage.pnumb_from_loc(key):
                if x in truck_list[1].packages:
                    myHashPackage.update_package_status(x, 'Delivered at {} by Truck 2'
                                                        .format(clock.show_time_delivered(value)))
                    truck_list[1].packages.remove(x)
    # Changes status for and removes packages that have been delivered by truck 3
    for key, value in truck_list[2].route.items():
        if total_miles_traveled() >= value:
            for x in myHashPackage.pnumb_from_loc(key):
                if x in truck_list[2].packages:
                    myHashPackage.update_package_status(x, 'Delivered at {} by Truck 3'
                                                        .format(clock.show_time_delivered(value)))
                    truck_list[2].packages.remove(x)


# Creates the truck class and functions
class Truck:
    def __init__(self, number, packages, mileage, route):
        self.number = number
        self.packages = packages
        self.mileage = mileage
        self.route = route

    # Prints the truck information
    def truck_number(self):
        print('Truck Number: {}\n    Packages: {}\n    Mileage: {}\n    Route: {}\n'.format(self.number, self.packages,
                                                                                            self.mileage, self.route))

    # Loads a truck with a package number
    def load_truck(self, package_number):
        self.packages.append(package_number)

    def add_route_locations(self, location):
        self.route[location] = 0


# Creates 3 truck objects with truck number, an empty list to populate with packages, and starting mileage of 0
truck_list = []
for i in range(3):
    truck_list.append(Truck(i + 1, [], 0, {}))


# Creates the clock object so I can show the time and package status
class Clock:
    def __init__(self, hours, minutes):
        self.hours = hours
        self.minutes = minutes

    def show_time(self):
        temp_clock_minutes = ''
        temp_clock_hours = clock.hours
        if clock.minutes < 10:
            temp_clock_minutes = '0{}'.format(clock.minutes)
        else:
            temp_clock_minutes = clock.minutes
        global pm
        pm = 0
        if clock.hours > 12:
            temp_clock_hours -= 12
        if clock.hours > 11:
            pm = 1
        return '{}:{} {}'.format(temp_clock_hours, temp_clock_minutes, clock.am_pm())

    def show_hours(self):
        return clock.hours

    def show_minutes(self):
        return clock.minutes

    def am_pm(self):
        if pm == 1:
            return 'PM'
        else:
            return 'AM'

    def show_time_delivered(self, distance):
        pm = 'AM'
        a = int(((distance // 18) + 8))
        if a > 11:
            pm = 'PM'
        if a > 12:
            a -= 12
        b = int(((distance % 18) / 18) * 60)
        if b < 10:
            b = '0' + str(b)
        if int(b) > 60:
            b -= 60
            a += 1
            if a > 11:
                pm = 'PM'
            if a > 12:
                a -= 12
        return '{}:{} {}'.format(a, b, pm)

    def advance_10_minutes(self):
        clock.minutes += 10
        if clock.minutes > 59:
            clock.minutes -= 60
            clock.hours += 1


def total_miles_traveled():
    total_hours_elapsed = clock.hours - 8
    total_minutes_elapsed = (total_hours_elapsed * 60) + clock.minutes
    return total_minutes_elapsed * 0.3


# Creates the clock object and assigns a starting time
clock = Clock(8, 0)

# Creates the distance data list and the hash table objects
myDistanceData = {}
myHashPackage = ChainingHashTable()
myHashAddresses = ChainingHashTable()

load_address_data('WGUPSDistanceTable.csv')
load_package_data('WGUPSPackageFile.csv')
load_distance_data('WGUPSDistanceTable.csv')

truck1_packages = [14, 15, 16, 34, 20, 21, 19, 4, 40, 1, 27, 35, 13, 39, 5, 37]
truck1_route = [20, 21, 17, 4, 18, 5, 1, 6, 19, 0, 24, 15, 13, 11, 26]

truck1_reload_packages = [25, 26, 31, 32, 6, 28, 22]

truck2_packages = [7, 29, 38, 8, 30, 3, 36, 12, 10, 2, 33, 17, 18, 23, 11, 24]
truck2_route = [2, 19, 12, 8, 7, 16, 25, 9, 14, 3, 23, 10, 22, 0]
# Package 9 has no deadline and can only be loaded after 10:20 AM so the driver of truck 2 will deliver it after
# returning from their route
truck3_packages = [9]
truck3_route = [12]

# Initial load of trucks
for x in truck1_packages:
    truck_list[0].load_truck(x)
    myHashPackage.update_package_status(x, 'EN_ROUTE / TRUCK_1')

for x in truck2_packages:
    truck_list[1].load_truck(x)
    myHashPackage.update_package_status(x, 'EN_ROUTE / TRUCK_2')

# Initial load of routes
for x in truck1_route:
    truck_list[0].add_route_locations(x)
for x in truck2_route:
    truck_list[1].add_route_locations(x)

# Adds the distance
cur_loc = 0
for keys in truck_list[0].route:
    truck_list[0].route[keys] = dist_locations(cur_loc, keys)
    cur_loc = keys
cur_loc = 0
for keys in truck_list[1].route:
    truck_list[1].route[keys] = dist_locations(cur_loc, keys)
    cur_loc = keys

# Changes distances in dictionary to cumulative distances
temp_value = 0
for key, value in truck_list[0].route.items():
    temp_value += value
    truck_list[0].route[key] = round(temp_value, 1)

temp_value = 0
for key, value in truck_list[1].route.items():
    temp_value += value
    truck_list[1].route[key] = round(temp_value, 1)

print('\nTime: {}\nTotal mileage: {}'.format(clock.show_time(), total_miles_traveled()))

inp = int(input('\n0) Exit: \n1) Display all packages: \n2) Display all trucks: \n3) Determine route: \n'
                '4) Set time: \n5) Look up package: \n6) Find 5 closest locations'))

while inp != 0:
    if inp == 1:
        package_update()
        # Split into 2 sections to make screenshots look better
        print('\n{} {}\n'.format(clock.show_time(), total_miles_traveled()))
        for i in range(len(myHashPackage.table) // 2):
            print(myHashPackage.search(i + 1))
        print('\n{} {}\n'.format(clock.show_time(), total_miles_traveled()))
        for i in range(len(myHashPackage.table) // 2):
            print(myHashPackage.search((i + 1) + (len(myHashPackage.table) // 2)))

    if inp == 2:
        package_update()
        for i in range(3):
            for keys, values in truck_list[i].route.items():
                max_miles = values
            if max_miles < total_miles_traveled():
                truck_list[i].mileage = max_miles
            else:
                truck_list[i].mileage = round(total_miles_traveled(), 1)
            if truck_list[2].mileage > truck_list[1].mileage:
                truck_list[2].mileage = round((truck_list[2].mileage - truck_list[1].mileage), 1)
            truck_list[i].truck_number()
        print('Total Mileage Combined: {:.2f}'.format(
            truck_list[0].mileage + truck_list[1].mileage + truck_list[2].mileage))

    if inp == 3:
        all_package_numbers = []
        for i in range(len(myHashPackage.table)):
            all_package_numbers.append(i + 1)

        # Package black list will make it so the packages at address don't show already delivered
        package_black_list = []
        # White list to prioritize deadline packages first, then specific truck packages (the grouped up packages
        # will go on truck 1)
        # 2 late deadline packages will go on third truck, the rest will be split in half between truck 1 and truck 2
        white_list = []
        # Preload visited list to blacklist locations
        black_visited_list = []
        a = set(all_package_numbers)
        b = set(white_list)
        c = black_visited_list
        # To use the white list, visited_list = d, to use the black list, visited_list = c
        d = list(a - b)

        visited_list = c
        temp_loc = 0
        print('\nDetermine Route...\n')
        inp_1 = int(input('Starting location?: '))
        while inp_1 != 'q':
            if -1 < inp_1 > 27:
                print('Invalid location')
                inp_1 = 'q'
                continue
            visited_list.append(int(inp_1))
            temp_loc = closest_location(inp_1, visited_list).split(', ')
            print(temp_loc[1])
            print(package_black_list)
            temp_pack_list = myHashPackage.pnumb_from_loc((int(temp_loc[1])))
            for i in range(len(temp_pack_list)):
                if temp_pack_list[i] in package_black_list:
                    temp_pack_list.remove(temp_pack_list[i])
                    temp_pack_list.insert(i, '--')
                else:
                    package_black_list.append(temp_pack_list[i])
            print('Dist: {},Location: {}, Packages at address: {}'.format(temp_loc[0], temp_loc[1],
                                                                          temp_pack_list))
            inp_1 = int(input('Starting location?: '))

    if inp == 4:
        hours_inp = int(input('\nSet Hour: '))
        if hours_inp > 12 or hours_inp < 0:
            print('\nInvalid Selection\n')
            continue
        minutes_inp = int(input('\nSet minutes: '))
        if minutes_inp > 59 or minutes_inp < 0:
            print('\nInvalid Selection\n')
            continue
        ampm_inp = int(input('\n1 = AM, 2 = PM'))
        if ampm_inp == 2 and hours_inp != 12:
            hours_inp += 12
        clock.hours = hours_inp
        clock.minutes = minutes_inp

    if inp == 5:
        package_update()
        myHashPackage.lookup()

    if inp == 6:
        inp2 = 'y'
        inp1 = int(input('\nStarting Location?: \n'))
        bad_list = []
        while inp2 == 'y':
            for i in range(5):
                cur_loc = inp1
                x = closest_location(cur_loc, bad_list).split(', ')
                print('Location: {}\nPackage(s): {}\nDistance: {}\n'
                      .format(x[1], myHashPackage.pnumb_from_loc(int(x[1])), x[0]))
                bad_list.append(int(x[1]))
            inp2 = input('\nGet next closest (y/n) ?: \n')

    print('\nTime: {}\nTotal mileage: {}'.format(clock.show_time(), total_miles_traveled()))
    inp = int(input('\n0) Exit: \n1) Display all packages: \n2) Display all trucks: \n3) Determine route: \n'
                    '4) Set time: \n5) Look up package: \n6) Find 5 closest locations'))
