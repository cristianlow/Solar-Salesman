# Solar-Salesman is a program that helps salesman find the shortest route to do his job
//Inland Empire Solar Salesman


#include <iostream>
#include <string>
#include <vector>
#include <algorithm> // Needed for finding different route variations

using namespace std;

// We Define The Cities
// We use a constant to tell the program we have 4 cities only
const int TOTAL_CITIES = 4;
string cityNames[TOTAL_CITIES] = {"Riverside", "Moreno Valley", "Perris", "Hemet"};

/* We us a weighted graph (Adjacency Matrix) ---
   This table stores the distance between cities.
   [0]=Riverside, [1]=Moreno Valley, [2]=Perris, [3]=Hemet
*/
int graph[TOTAL_CITIES][TOTAL_CITIES] = {
    // Riv,  MoVal, Perris, Hemet
    {0,     16,     24,     33}, // Distances from Riverside
    {16,    0,      18,     26}, // Distances from Moreno Valley
    {18,    18,     0,      30}, // Distances from Perris
    {33,    26,     30,     0}   // Distances from Hemet
};

int main() {
    // We store the 3 cities they need to visit after leaving Riverside.
    // Index 1 = Moreno Valley, 2 = Perris, 3 = Hemet.
    vector<int> destinations = {1, 2, 3};

    int shortestDistance = 1000000; // Start with a huge number to be safe
    vector<int> bestRoute;          // To save the winner

    cout << "Calculating all possible trip variations starting from Riverside..." << endl;
    cout << "-----------------------------------------------------------------" << endl;

    /* We evaluate the variations of the route.
       The specialist starts at Riverside (Index 0).
       We use a 'do-while' loop and 'next_permutation' to try every 
       possible order of the remaining cities.
    */
    do {
        int currentTripDistance = 0;
        int startCity = 0;             //We must always start at Riverside

        // Show the current route we are testing
        cout << "Route: Riverside";

        for (int i = 0; i < destinations.size(); i++) {      // Loop through the cities
            int nextCity = destinations[i];

            // Look up the weight/distance in our Weighted Graph (Matrix)
            // graph[startCity][nextCity] gives us the distance
            currentTripDistance += graph[startCity][nextCity];

            // Move the specialist to the next city for the next step of the loop
            startCity = nextCity;

            cout << " -> " << cityNames[nextCity];
        }

        cout << " | Total Distance: " << currentTripDistance << " miles" << endl;

        /* We must find the shortest and cheapest trip.
           If this trip is shorter than our previous 'shortestDistance', 
           we save it as the new champion.
        */
        if (currentTripDistance < shortestDistance) {
            shortestDistance = currentTripDistance;
            bestRoute = destinations;
        }

    } while (next_permutation(destinations.begin(), destinations.end()));

    // We display and output the results
    cout << "\n-----------------------------------------------------------------" << endl;
    cout << "RESULTS FOR THE SOLAR MARKETING SPECIALIST:" << endl;
    cout << "Shortest Path: Riverside";
    for(int cityIndex : bestRoute) {
        cout << " -> " << cityNames[cityIndex];
    }
    cout << "\nTotal Distance: " << shortestDistance << " miles" << endl;
    cout << "Most Low-Cost Trip: Same as shortest path (minimizes fuel/miles)." << endl;

    return 0;
}
