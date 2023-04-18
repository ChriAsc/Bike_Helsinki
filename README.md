# Social Network Analysis applied to a bike-sharing service in the city of Helsinki through the use of the NetworkX library

## Introduction
### 1.1 Social Network Analysis

Social network analysis is the process of investigating social structures
through the use of graph theory and represents one of the most
important branches of Data Science. According to social network theory, society is
seen as a network of relationships and Social Network Analysis studies both the
individuals and the structure of relationships between them. SNA has its roots
roots several years before the advent of social networks (such as Facebook or
Twitter) and in particular in the field of sociology, with the aim of studying
the social relationships that are created between people. Since it is based on the theory of
graphs, SNA has to consider certain elements: nodes (people, devices
objects) and the arcs (the relationships between the nodes, such as friendship, hatred). By mapping
a network onto a graph, important properties can be analysed, such as
the influence of components, the cohesion between nodes and the presence of patterns
interesting patterns. Social Network Analysis is also used in the field of
transport, as the abstraction allows attention to be focused on the structure
of the system itself. Therefore, individual geographical points are identified as
nodes, while routes are identified as arcs. Usually, this type of network has
physically constrained arcs (such as railways or roads), while other types of
transport (such as bike-sharing) are free of such restrictions.

### 1.2 Helsinki City Bikes Dataset

The dataset on which we chose to base the entire project was the Helsinki
City Bike Dataset (https://www.kaggle.com/datasets/geometrein/helsinki-city-bikes);
the latter contains a collection of all the trips made by the
citizens of the Finnish capital using the public bicycle service.

Each trip is characterised by a set of general information such as:

- **Departure** : day (dd-mm-yyyy) and time (hh:mm:ss) of departure
- **Return** : day (dd-mm-yyyy) and time (hh:mm:ss) of arrival
- **Departure_id** : numeric identifier of the departure station
- **Departure_name** : name of the departure station
- **Return_id** : numeric identifier of the arrival station
- **Return_name** : arrival station name
- **Distance (m)** : distance in metres travelled during the journey
- **Duration (sec)** : Journey time in seconds
- **Avg_speed (km/h)** : average sustained speed
- **Departure_latitude** : latitude of the point of departure
- **Departure_longitude** : longitude of the point of departure
- **Return_latitude** : latitude of the point of arrival
- **Return_longitude** : longitude of the point of arrival
- **Air temperature (degC)** : air temperature measured during the
    journey in degrees Celsius.

Bike-sharing, unlike other transport services, is by its nature more
flexible, not least because it depends on the needs of those using the service.
Therefore, the arcs in the network are not predetermined, but are generated
by the users. The structure of the network takes shape from the very flow of those who
who use the service and, as a result, particular patterns can be found from
from repeated trips over time. In addition, the increase or decrease
of service use can be key elements in gaining more
information on certain areas, both geographically and socially.

## 2. Network analysis

### 2.1 Pre-processing

After downloading and importing the dataset into our development environment, it was
it was immediately clear that operating on it would be complicated
computationally complicated (a size of approximately 2 GB). Therefore, as a first
operation, the size of the dataset was reduced,
selecting only the data for the year 2020 and excluding the previous ones.
earlier data; this simplification resulted in a smaller number
of data, from more than 12 million records to just 3 million.

Subsequently, the **_departure_** and **_return_** fields were converted from a
String format to Datetime format, which is more convenient to use. Having done this, we
proceeded with the data cleaning operations, considering only records
containing values not too far from the averages. After eliminating data
containing null values, filters were applied to the most relevant attributes (Figure 1).

We chose to consider trips characterised by a length
between 5 0 metres (removing the least significant trips) and 30 kilometres
(removing trips that were too long). Subsequently, we also discarded
records characterised by a duration outside the range from 3 minutes
(very short trips) to 12 hours (too long trips) and those in which
out-of-range temperatures above 40 degrees or below - 20 appeared.

### 2.2 Graph description and metrics

Once the transformation operations have been completed, using the method
_from_pandas_edgelist_ method of the NetworkX library, an
indirect graph from the dataframe produced during the previous steps.

Exploring the information of the newly generated graph, it was seen that
the latter was composed of **347 nodes** and **31,693 arcs** (Figure 2).

At this point, the structural characteristics of the network were considered, so as to
to understand what the specific properties were; in particular:

- **Density:** calculated as the probability that any pair of
    nodes are adjacent, i.e. it measures how connected the nodes are.
- **Excentricity:** for a node, it is the greatest possible shortest path between
    that node and all others.
- **Diameter:** indicates the greatest shortest path between a pair of nodes in the
    graph; hence, it is the maximum possible eccentricity value of a node.
- **Radius:** is the minimum eccentricity value of a node.
- **Periphery:** is the set of nodes that have their eccentricity equal to the
    diameter of the graph.
- **Centre:** is the set of nodes which have their eccentricity equal to the
    radius of the graph.
- **Average Clustering Coefficient:** is the average of the local
    local clustering of each node in the graph, which measures how much its
    neighbours tend to form a _clique_; therefore, it indicates how much the nodes
    tend to be connected to each other.

The density is around 0.5, so the graph is very
dense. With regard to eccentricities, these take on only two possible values:
2 or 3; consequently the diameter corresponds precisely to 3, while the radius to
2. This testifies that many nodes are directly connected and, in fact, there
34 stations represent the periphery of the graph, while the remaining
nodes correspond to the centre. As for the average coefficient of
clustering, the value is approximately 0.75, so 3/4 of the stations, which are connected
with a station, are also connected to each other.

## 3. Data visualisation

## 3.1 Standard visualisation

In order to have a graphical visualisation of the network, we took advantage of some
layouts made available by the NetworkX library. Therefore, we first
considered the Spring Layout, in order to understand how the network was organised;
as can be seen in Figure 3, there is a densely populated central area,
while in the outer part, the nodes are chaotically arranged.
Thus, no peculiarities or any significant groups can be discerned from this.

Another visualisation is provided by the Spiral Layout: in Figure 4, in the
centre there are nodes that are very close together, while on the branches of the spiral the nodes
are further apart; however, despite the distance from the centre, the arcs
remain dense. This confirms the density of the network, however, one cannot
to extract further information.

### 3.2 Geographical visualisation and map

In contrast to many network types, the layouts provided by the
NetworkX library are often not useful in the geographical context.

Since stations are physically bound to a certain location in the
territory, one can visualise the nodes according to the distribution
geographical distribution, mapping them with the corresponding longitudes/latitudes. Indeed,
via a dictionary (name: position), each individual node is associated
of the graph with the corresponding coordinate, according to a certain scale in order not to
make the nodes excessively scattered. In this way, it is possible to have a
more precise overview of the bike-sharing stations and consequently
extrapolate additional information. In Figure 5, it can be seen that there are
areas with different densities of arches, which indicate the presence of a
central area and peripheral areas.

For an even more precise visualisation, the network can be represented on a
a map in order to reveal other special features. In the map of
Helsinki (Figure 6), a station is depicted by means of a circle, whose
size depends on its presence in the dataset. As before, we
areas with larger circles, synonymous with greater transit at those stations, and other areas with larger circles.
those stations, and other areas with smaller circles.

In fact, the central component, in which there are strongly connected nodes
to each other, corresponds to the city centre of Helsinki, while the other areas
represent the periphery of the city.

As one might expect, in the centre of Helsinki (Figure 7) there is a
great activity in terms of mobility (in this case bike-sharing),
since economic, social and cultural activities are concentrated here.

However, the centre of Helsinki does not necessarily correspond to the centre
of the network, so it needs to be analysed in more depth.
In fact, more complex networks are highly heterogeneous, so often
some parts provide more information.

## 4. Centrality analysis

Centrality metrics in social network analysis serve to
provide us with information regarding the importance of each node, given its
position within the network. We can distinguish different types of
centrality, and each of them refers to different properties. In the context
of transport, an urban region in which the largest flows of
of users could be considered central, but often the concept of centrality
centrality changes from context to context and/or with changing conditions.

In our analysis, the following were examined: _Degree centrality,
Betweenness centrality, Eigenvector centrality and Closeness centrality_.

### 4.1 Degree centrality

Degree centrality represents the simplest measure of centrality, as
as its value indicates the number of nodes adjacent to the node we are
being considered. Although it is a very simple measure to calculate, it does not
give us information regarding the importance of the node related to the weight of its
connections. In the context of bike-sharing, this measure refers to the
number of stations to which users have travelled from a particular
particular station.

As can be seen in Figure 8, the distribution follows an
almost Gaussian trend; this is due to the fact that stations in the
city centre have a high number of connections with neighbouring stations,
while as one moves to the periphery, stations tend to have fewer and fewer adjacencies.
fewer and fewer adjacencies.

After locating the area with the highest degree centrality in Figure 9, one can
the nodes of the graph can be analysed in detail. In fact, it can be seen that the
ten stations with the highest degree centrality are:
- Haukilahdenkatu: 0.9161849710982658
- Paciuksenkaari: 0.815028901734104
- Itämerentori: 0.8092485549132947
- Laajalahden aukio: 0.8092485549132947
- Huopalahdentie: 0.8092485549132947
- Munkkiniemen aukio: 0.7890173410404624
- Ympyrätalo: 0.7803468208092486
- Töölöntulli: 0.7803468208092486
- Tilkanvierto: 0.7745664739884393
- Paciuksenkatu: 0.7687861271676301

Since the Haukilahdenkatu station is located in the first position, it can be
assert that a large number of citizens, also from different
districts, run regular routes to and from this station. This could
indicate the presence of a focal point or point of special interest.

### 4.2 Betweenness Centrality

This second type of centrality is intended to measure the extent to which a
node acts as a compulsory crossing point between two portions of the network. Given
a node _x_ , the shortest path is calculated for each pair of nodes in the graph;
Once this is done, the number of times _x_ appears in these paths is counted: the more
higher this value, the more node x will represent a "bridge" between two parts
of the network.

Looking at Figure 10 showing this measure, one can see a concentration
of nodes on the lowest values. This phenomenon is attributable to the nature
nature of the network itself, which, having 31,693 arcs and only 347 nodes, is
extremely interconnected and - as a result - it is difficult to find nodes with high betweenness values.
with high betweenness values.

Figure 11 shows only one node with a betweenness centrality
of a certain magnitude, corresponding to the station of Haukilahdenkatu
(0.01677); this indicates that even if this station were to be missing,
alternative routes would still be found without causing great inconvenience.

### 4.3 Eigenvector Centrality

This centrality measure provides us with a value of node influence
within the network, based on its importance in relation to the nodes
surrounding it. The measure is calculated by following an iterative process
during which connections with nodes having a high coefficient of
eigenvector centrality raise the coefficient of the reference node; conversely
contrary, adjacencies with nodes having a low coefficient provide lower contributions
smaller contributions.

In our case, it is possible to observe how the distribution (Figure 12) of
this measure follows the trend of degree
centrality. This behaviour can be traced back, as in the case
previous case, to the higher density of links in the inner part of the network
(corresponding to the city centre). On the other hand, for the same reason there is
also a lowering of the average values of eigenvector centrality.

Also in Figure 13, a substantial similarity with degree
centrality; in fact, the ten stations with the highest eigenvector centrality are:

- Haukilahdenkatu: 0.08017456245366736
- Paciuksenkaari: 0.07660782649717626
- Huopalahdentie: 0.07640991844763213
- Laajalahden aukio: 0.07582533040235645
- Töölöntulli: 0.07577013109459187
- Itämerentori: 0.07564122953604463
- Ympyrätalo: 0.07556224884855904
- Tilkanvierto: 0.07537363940444101
- Pasilan asema: 0.0751848451773669
- Linnanmäki: 0.07473912538258863

In a network concerning public transport, eigenvector centrality allows
to emphasise not only the importance of individual nodes, but also areas
within cities that may have a certain relevance. Indeed,
the importance of a station derives mainly from its relationship with other
nodes, rather than from geographical characteristics (location, size).

### 4.4 Closeness Centrality

The last measure of centrality we evaluated was closeness centrality.
For each node, it provides a value indicating the ability of that node to
distribute information in the network. It is measured by calculating the
average distance of the node from all others in the network. In our case, a node
characterised by a high closeness centrality value indicates a station
that could act as an intermediary for other stations. From Figure 14,
it can be seen that the highest concentration of points is in the area of the
medium-high values of closeness centrality. This phenomenon can be
motivated by the fact that the public bicycle system was studied
with precision, paying particular attention to the optimisation of the
geographical distribution of the stations.

In fact, looking at the map of Helsinki, one can see that the stations
evenly distributed throughout the entire city area, leaving no empty areas between the
without leaving any empty areas between one point and another. In the context of bike
sharing, users could be motivated to make stops at intermediate
intermediate stations in order to avoid higher payments.

As visible in Figure 15 and as described above, a large number of
of stations are directly connected to Haukilahdenkatu and this node is
characterised by the highest closeness centrality (0.9177718832891246).
As can be expected from the structure of the network itself, the closeness centrality
of the other stations follows the same trend as the degree centrality.

## 5. Structure analysis

Another very interesting aspect belonging to the field of social
network analysis concerns the study of the substructures of the network.
It is possible to define many types of sub-graphs and each of these provides us with specific
specific information about the network such as robustness, cohesion or
interconnection. In this project, we focus mainly on the
concepts of _Community_ and _Triad_.

### 5.1 Community detection

One type of substructure of interest in the analysis of a network is
represented by communities. In an intuitive manner, these
can be defined as subsets of nodes of the network, between which
there is a denser interconnection than in the rest of the graph. With regard to
bike-sharing, the identification of communities can help understand
possible patterns in the use of the service or to focus on elements of an
economic nature (such as price adjustment).

The process of community detection is done through the creation of a
a tree structure called a dendrogram, in which the leaves
represent the nodes of the network and the arcs are used to connect nodes or groups of nodes, in fact creating a dendrogram.
of nodes, effectively creating a hierarchical structure of grafted communities (Figure 16).
grafted (Figure 16).

Applying this procedure to the network of bicycle stations, as a
result we obtained a list of arrays representing the various
communities found. The largest of these subsets contains 88
stations while the smallest have less than 10 elements.

The presence of such a large community within our network is
further proof of how well-designed this infrastructure is.
ensures a highly capillary distribution of stations. Figure 17,
shows the community described on the map of the city of Helsinki.

Since the NeworkX library provides different solutions for the search of
communities, the Louvain method was applied, which is
simple and straightforward. In fact, it is a heuristic method based on two
steps: in the first step, each node represents a community and nodes are assigned
nodes to neighbouring communities only if a positive gain of
modularity (the end is reached when there is no improvement in the
value of modularity); in the second phase, a new network is built
where each node is part of the previously assigned community.

Applying the _louvain_communities_ function to the network, we obtain four
distinct communities, identified by the colours blue, green, yellow and pink (Figure 18).
This means that cycling within these communities is higher than cycling between them.
higher than that between them. In addition, it can be seen that the nodes with
higher betweenness centrality correspond to border areas between
different communities, confirming previous observations.

From a geographical point of view (Figure 19), it is interesting to note that the
communities echo the spatial subdivisions of Helsinki and that therefore the
presence of the sea causes the separation of the communities themselves (in fact, there is an
archipelago in the south-west). This also testifies to the presence of physical constraints
in the development of the bike-sharing infrastructure.
### 5.2 Triads

In an oriented graph, a triad is understood to be a group of 3 nodes A, B and C between which
between which various combinations of links may exist. The way in which
these three nodes are connected provides information regarding the network's ability
of the network to disseminate information and regarding the robustness coefficient.
Taking the closed triad as an example (in which all 3 nodes are
connected), it represents the ultimate form of robustness, as if
even one of the 3 links were to fail, the information would still be able to
still be able to reach all 3 points.

In particular, considering a direct graph, it is possible to define sixteen
types of interconnection between nodes A, B and C; each of these
combinations is characterised by an identification code. In Figure 20,
below, we link each conformation to its code.

In order to assess the numerosity of each of these triads within the network, it was
it was necessary to derive an oriented graph. This operation is carried out by the
command _from_pandas_edgelist_ (already used previously) within which
which must specify that the arcs are marked by a direction,
by passing the attribute _create_using_ the class "DiGraph" (Figure 21).

Having obtained our directed graph (Figure 22), to count all occurrences of
triads we exploited the _triadic_census_ method of the NetworkX library
library (Figure 23). As a result, we obtain a dictionary that binds to each code
an integer value, representing the numerosity found in the network of the
triad in question.

Looking at the results (Figure 24), it is impossible not to notice the high number
triads found; these values are so high due to the large number of arcs present in our network.
number of arcs present in our network. In particular, it is interesting
to observe the strong presence of complete triads (61 thousand instances of the triad
"300", i.e. completely closed), which emphasises the strong
interconnection and robustness that characterise this network of stations.


## 6. Conclusions

Through the use of the NetworkX library, it was possible to analyse a complex network,
such as that described by the bike-sharing service in the city of Helsinki.
At first, a study of the dataset was performed in order to understand the size of the network and the number of stations.
Once the network was represented on an indirect graph, key metrics were calculated in order to have a more precise overview.
For this very reason, a geographical visualisation was used and
the correspondence of the nodes on the Helsinki map made it possible to detect previously undetected peculiarities.
At this point, the centralities were studied and the most important stations were considered.
In conclusion, the structure analysis was carried out, with a focus on community detection and triads:
in the first case, two different methods made available by the Python library were used,
finding various communities and their correspondences on the map;
in the second case, a direct graph was derived from the same dataset and the triads detected in it were studied.

#### _Disclaimer: in-text images refer to 'Relazione_BikeHelsinki.pdf' file_
