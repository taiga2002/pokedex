# Pokedex

The goal for this project is to create a Pokedex ios app using [PokeAPI](https://pokeapi.co/)
to display a list of pokemons and select your favorite pokemon! (Swift UI)


## Roadmap

- MileStone 1: Create a MVP that fetches a list of pokemon using this [Api](https://pokeapi.co/api/v2/pokemon?limit=20&offset=0) in a grid shape and show it after you tap a specific pokemon

- MileStone 2: Add pagination to load additional 20 pokemons as you reach the bottom of the screen to minimize the data per request (API limit=20)

- MileStone 3: Add image caching in different persistent layers and options.

- (Extra) MileStone 4: Polish the UI by adding some coloring, shape and animation for searchbar.

- (Extra) MileStone 5: Add vibration and sound effect when you press a pokemon.


## Directory Strcture
```bash
── Pokedex
    ├── Sound
    │   ├── pokemon_buzz.mp3
    │   ├── pokemon-button.mp3
    │   ├── pokemonCenter.mp3      
    ├── Utility
    │   ├── DataHandler
    │   ├── PhotoCache
    │   ├── FilePhotoManager
    │   ├── HapticManager    
    ├── Data
    │   ├── pokemon.json    
    ├── Models
    │   ├── Pokemon     
    ├── ViewModels
    │   ├── PokemonListModel
    │   ├── ImageViewModel 
    ├── View
    │   ├── ContentView
    │   ├── PokemonHeaderView
    │   ├── ImageView
    │   ├── PokemonView  

```
## MileStone 1

### Steps

(1) Created MVP basic UI with hardcoded data. This includes creating a subcomponent of PokemonView to display in each grid, loading image with AsyncImage and name for erach pokemon for PokemonView, and display in a scrolable grid form.

(2) Defined PokemonPage and Pokemon decodable model to decode json to each model

(3) Created decode function in dataHandler method to test if decoding json into each model works or not, using pre-downloaded json data.

(4) Created HeaderView to display a pokemon passed into this view.

(4) Added onTapGesture for each PokemonView and added state variable to keep track of which pokemon is selected, and pass that into HeaderView.

(5) Added ObservableObject PokemonListModel to load in pokemon data from actual URL using Combine

(6) Use PokemonListModel to retrieve data and dispaly in a grid

### Decision Making/ Thought Process

- Using GeometryReader vs UIScreen.main.bounds.height 
---> Geometryreader is generally better in terms of flexibility in different portrait. Using generic height/width of UI could mess up the design in landscape mode.
- Why Combine in stead of async/await method to fetch data?
---> In Apple’s words, Combine is a declarative Swift API for processing values over time. Combine extends functional reactive programming by embedding the concept of back-pressure. Back-pressure is the idea that the subscriber should control how much information it gets at once and needs to process. This leads to efficient operation with the added notion that the volume of data processed through a stream is controllable as well as cancellable. https://heckj.github.io/swiftui-notes/

- How would I get pokemonId when json value doesn't have attribute called "id"?
---> Since the index for each pokemon in results list is equal to the unique id for each pokemon, simply determined in which index a pokemon exists in the list

## MileStone 2

### Steps 

(1) Added one function to PokemonListModel in order to handle different offset number and update pokemonList by appending additional pokemonList

(2) Added a fake button at the end of scrollView to see whether it adds more pokemon and updates view correctly.

(3) Replaced a fake button by adding an empty view at the bottom of scrollView using onAppear in order to retrieve additional 20 more pokemons by adding offset by 20 everytime when you reach the bottom of the scrollView.

### Decision Making/ Thought Process

- How am I able to know if I have reached the buttom of the scollView?
---> I was first a bit stuck about pagination, especially about how I can automatically call the function to retrieve data at the bottom of the screen. I first thought of a method in which you have to calculate the offset height difference using geometryReader, yet I realized having an emptyView and use .onAppear to trigge a call is a much easier method.

- Is limit of 20 as specificed in the spec a reasonable number?
---> I first thought it made sense to request a small amount of data and request more if necessasry. Yet, as I scroll the pokemon list very fast, I realized that there is some latency displaying the pokemon, and I feel like increasing the size of each limit to 30-40 might be a better user experience.


## MileStone 3
### Steps

(1) Added Kingfisher library to a project, and used it for both in-memory as well as disk cache.

(2) Created PhotoCache (NSCache manager) for in-memory cache for saving images

(3) Create FilePhotoManager for disk cache, by storing images in local file memory

### Decision Making/ Thought Process
- Why Kingfisher? 
---> Kingfisher will download the image from url, send it to both memory cache and disk cache, and display it in imageView. When you set it with the same URL later, the image will be retrieved from the cache and shown immediately. Since it handles such a various aspects of caching, it really becomes handy once you download the library.
- When to use NSCache?
---> When I was building the NSCache, I assumed that for the purpose of this app, it's reasonable to use in-memory database as a lot of images displayed would be something like scrolling through instagram profile pictures.
- How should I set the custom countLimit and totalCostLimit?
---> For countLimit, I assumed that each request would give us 20 pokemons and scrollign to bottom 5 times is quite a lot of pokemons, so refreshing after storing 100 images is a reasonable number. For totalcostlimit, as it is in bytes, logic is 100 X (1024 kB / 1 MB) X (1024 bytes/ 1 kB).
- When to use FileManager Cache? 
---> Although adding all the pokemon images to user's local storage is a good idea, this has some scalability in the future when you add other functionalities such as managing your own Pokemon and display on your profile screen. Since those persronal information tend to be accessed more than a list of random pokemons, it makes sense to use disk cahche for certain data.
- Where should I store cache images in local disk on ios device?
---> As I have researched, those three options(FileManager.default, manager.temporaryDirectory, .cachesDirectory) sounded like aviable options, as the app container provides two directories for storing purgeable data:
both of "/tmp" and "/Library/Caches". (Purgeable data is app data that the system can delete without affecting the app’s execution and that the app can re-create if it needs. )

(https://developer.apple.com/documentation/foundation/optimizing_your_app_s_data_for_icloud_backup/)






## MileStone 4

### Steps
(1) Added linear gradient for background color for both header and each pokemon view by adding custom color

(2) Added shadow effect and round corner for each Pokemon view

(3) Added custom font to change the text font

(4) Added searchbar functionality by adding a function to filter out pokemons by searchText, and added animation to filter out pokemonView

(5) Adjusted spacing

### Decision Making/ Thought Process
- How did I decide color schema? 
---> I first thought I would go with basic color like gray/black/white and create more stylish design, but as I tried different color schema, I realized birght colors attract more attention and would look more like pokemonApp. I decided to use lineargradient for aethetics, and added a custom color to create my own design.


## MileStone 5
(1) Added customizable sound effect so that when you press pokemon, you get a sound effect.

(2) Searched favorite 3 pokemon sound online as mp3 format. (My favorite is pokemon-center!! reminds me of childhood :) )

(3) Added vibration effect (you can only try on physical ios device so tested on my iphone)
