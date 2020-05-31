# Composition-Compass

The following is a documentation to **"Composition Compass"**. At its core, it supports the following features:

1. Scrape song informations from Pandora stations/Spotify playlists

2. Download all those songs automatically from YouTube (as batch download)
    
    2.1 Alternatively download a specific artist, a specific album or similar songs to a specific one from YouTube (doesn't have anything to do with Pandora, as the songs will be downloaded based on Last.fm informations)

3. Organise the downloads into local folders (one for each scraped station, artist or album). "Similar tracks" are treated as stations here.

3. Play the downloaded songs in "Carmode": You can play a downloaded station folder via Bluetooth and just press Vol+ or Vol- keys (without even looking on your phone) to move the currently playing track to a 'favorites' or ' recycle bin' folder.

4. Play the favorites folder in carmode as well. Here Vol+ and Vol- move a track to "More interesting" and "Less interesting" sub-folders.

5. Also do a "normal" playback which allows to move songs to the corresponding folders by the means of a graphical user interface rather than the volume buttons

&nbsp;

## Installation

1.	Install Tasker from Play Store

    1.1	Install Scroball from Play Store and login to Last.fm
    
    1.2	Install TouchTask plugin for Tasker from Play Store
    
    1.3 Install Musicolet from Play Store
    
    1.4	Install Drive Sync from Play Store
    
    1.5 Install Spotify from Play Store
    
    1.6 Install Pandora and Orbot
    
      1.6.1 Pandora from Play Store or external source, depending on Pandora's availability in your country.
      
      1.6.2 **Composition Compass only supports the use of Pandora through TOR**. If you life in a country where Pandora is accessible without a proxy just delete the "Launch App -> Orbot" action in the start task of Pandora Scraper project.
      
    1.7	 Install Termux from Play Store
    
      1.7.1  Run 'termux-setup-storage'
      
      1.7.2  Copy "install_youtubedl.sh" to Termux home directory and run
      
&nbsp;

2.	Restore Composition Compass backup data ("backup.xml")
    
    2.1	Copy "Pandora" folder to "/storage/<external_sdcard_name>/Android/data/com.termux/files"

&nbsp;

3. Open configuration ("config.ini"). For a detailed explanation see "Pandora folder" section.

    3.1 Add a Last.fm API key
  
    3.2 Define track exceptions
          
&nbsp;
  
4. Grant necessary permissions to Tasker
  
    4.1 Grant notification access
  
    4.2 Grant accessibility service
  
    4.3 Grant usage access
  
    4.4 Grant all other requested permissions as well
    
&nbsp;

5. Add shortcut to 'FrontEnd' task to home screen.
    
    5.1 Launch shortcut and enter path to sdcard root.

&nbsp;


## Features

The following is an explanation of the functionality offered after launching the FrontEnd.


#### Scrape Pandora
Start a new scraping process for Pandora. The Pandora app will be repeatedly opened and closed, and song meta-informations be scraped during this process. Output can be found in "Pandora/song_log.csv".
This requires you to set up a Pandora station (inside the Pandora app) *before* running 'Scrape Pandora', as only the current station will be scraped.

Scraping title and artist informations is based on Logcat entries of Scroball. Therefore ensure the app is set up properly before launching the Scrape Pandora task. You just need to be logged in to Last.fm inside the Scroball app, afterwards Composition Compass shouldn't have any issues fetching track meta data.
You can always cancel this task by pressing Home Button.




#### Scrape Spotify
Start a new scrapping task for Spotify. Instead of repeatedly opening and closing the app though, this is done by the means of skipping through the currently opened item (e.g. Playlist, Album or Radio), which you need to have opened before launching this task as well. As it is for Pandora, the fetched informations will be stored in 'song_log.csv'.

I don't recommend using the Spotify Scraper for scraping a Spotify Radio. Pandora is pretty much the same, and it's got the Music Genome Project as backing. If you want to know what this is look it up yourself, but essentially this means Pandora's recommendations should have a way stronger tendency to be what you're *actually* looking for. I would also confirm this from my own experience.




#### Download [item]
Composition Compass offers multiple download options. Each of those is available after pressing the 'Download Options' button on the FrontEnd, and allows to download the song_log.csv, albums, tracks by artists or similar tracks from YouTube.

For a download, Termux will be opened and the necessary youtube-dl command be pasted and executed. The songs will be downloaded as opus files to a directory which depends on the chosen download option.


1. <ins>For item 'Song Log'</ins>  
Downloads songs based on metadata found in "song_log.csv" to "Pandora/Downloads".


2. <ins>For other items</ins>  
All other download options are based on Last.fm informations aquired through its API. There are several options to download tracks by artist, by album or by similarity. The 'Download Similar' option can be seen as third 'Radio Source' next to Pandora and Spotify, and might therefore be of special interest to you.

  Albums and similar tracks will be placed in "Pandora/Playlists". Tracks by artists in "Pandora/Artists", where each artist will be downloaded to a dedicated folder.




#### Create Station
Creates a new "station" based on the current tracks in "Pandora/Downloads" directory. This station is a sub-folder of "Pandora/Stations" containing all songs of "Pandora/Downloads". You will be be prompted for a station name beforehand, that is going to be the folder name.

Remixes, nightcores and dubstep mixes will be moved to "Pandora/.Automated/Remixes" and the original "Pandora/Downloads" folder be cleared once the station is created. The song_log.csv file will be moved to "Pandora/Logs/<station_name>.csv".




#### Sort Out Station
Removes all tracks in the specified "affected station" which are already present in the specified "filter-station".

Intended to be used to remove duplicates after a download of new tracks. "File not found" errors are to be expected whenever a track of "filter-station" isn't found in "affected station", and therefore normal.




#### Sync Logs
Opens "Drive Sync"-app and starts synchronization of the configured mappings between local and remote directory of Google Drive. Benefit of this feature is questionable, but it has been implemented in an attempt to add an integrated synchronization of "Pandora/Logs/"-folder with a folder on Google Drive. Both folders need to be configured in Drive Sync before. Possibly useful if BlueStacks is used to scrape song informations as well.




#### Listen to...
Asks for an item to listen to, and opens Musicolet along with a local "Station Control" user interface afterwards


The following **items** are available:

1. <ins>Station</ins>  
This prepares a station folder inside "Pandora/Stations" for playback. Within Musicolet the previously selected station folder needs to be opened afterwards.


2. <ins>Favorites</ins>  
This prepares the "Favorites" folder for playback. Within Musicolet this folder needs to be opened afterwards.

3. <ins>Anything</ins>  
This allows to listen to anything, however liking and disliking will be disabled.


The **interface** generally allows to...

1. <ins>Show "Artist Info"</ins>  
Show an "artist pane" on the left side of the screen, which gives you a short biography about the currently playing artist and the styles usually associated with that artist. Tapping on the biography text exposes Composition Compass' download options and allows you to perform a Google search query for the artist, as well as to open the artist's Last.fm page.


2. <ins>"Like" and "Dislike" tracks</ins>  
In "Station" mode (see above) by "liking" a track, the currently playing song will be moved to "Pandora/.Automated/Favorites", so it can be easier found later on. By "disliking" a track the currently playing song will be moved to	"Pandora/.Automated/Recycle Bin" folder. Liking and disliking can happen by the means of physical volume buttons ("Car-Mode") or virtual UI buttons ("Home-Mode").

    In "Favorites" mode tracks will be moved to "More interesting" and "Less interesting" subfolders (of the "Favorites" folder) likewise.

    In "Anything" mode liking and disliking will be disabled. It's just useful if you want to use the artist info pane on its own.


3. <ins>"Restart" and "Stop" the station</ins>  
Stop to disable volume button triggers and hide the UI, restart to quick fix a bug of Musicolet that might lead to your car radio not showing song meta data (instead you'll just see "Not provided").


4. <ins>"Minimize"</ins>  
This button minimizes the UI to a bubble on the right hand side of the screen. Note: This bubble is *not* movable.




## Pandora folder

The following files and folders are available inside the Pandora folder:

#### config.ini
This is a configuration file for Composition Compass. Two keys are relevant for you:


1. <ins>Title exceptions</ins>  
The value to this key is a regular expression indicating which YouTube videos you *don't* want to be downloaded. If a video title contains any keyword defined in this expression it will be ignored.

    You can use this key to e. g. add names of bands you already know and names of bands you don't like. You can also blacklist remixes and the like if you want to.  

2. <ins>Last.fm API key</ins>  
Insert yourAPI key here. Required for fetching song / artist informations from Last.fm.


Other keys shouldn't be of interest for you, look them up yourself if you want to know what they're supposed to do.


#### Logs
A place to permanently store previously fetched "song_log.csv"-files. As those logs can be imported into Excel quite easily, and to have a low-size backup of your scraped songs (aka their metadata), you might want to backup this directory. See "Sync Logs" feature described above.


#### Stations
Folders for each Pandora station / Spotify item songs were fetched from, containing the actual audio files. Those folders serve as base for "Listen to..." feature.


#### .Automated
Contains "Favorites" and "Recycle Bin" folders relevant for "Listen to..." feature. Also "Remixes" folder relevant for "Create Station" feature.


#### Downloads
Contains the YouTube rips of the *current* download from YouTube (will be filled by "Download Song Log" feature).
