# Understanding performance in Arma 3

### Why it's not great

Arma 3 is built on Bohemia's Real Virtuality engine, and contrary to popular belief, it does actually utilize multithreading for many things. However it still suffers from severe performance bottlenecks in critical aspects due to limitations in its design. This is why even the most modern, high-end systems sometimes struggle to run Arma 3 at high frame rates 10+ years after its release.


## Choice of hardware

**CPU**

Arma 3 is very responsive to CPU core performance, as well as cache size. Nowadays it is possible to get very good out-of-the-box frame rates using a CPU which utilizes AMD's 3D V-Cache technology - these CPUs have significantly more on-board memory (cache), which is used to store frequently accessed data. The more cache memory a CPU has, the more performance-sensitive data can be stored in it, as opposed to fetching it from RAM, which is significantly slower by comparison. 

As mentioned above, an AMD CPU with 3D V-Cache is *highly* recommended for your next PC build, or upgrade. These CPUs not only greatly improve performance in Arma 3, but also almost every other game. Even if you are on an older AM4 socket system, you can still reap the benefits of this technology with a 5800X3D, 5700X3D or 5600X3D.

**RAM**

Just like CPUs, Arma 3 is very responsive to RAM performance. Only a comparatively small amount of data can be stored in CPU cache, so having a kit of RAM which is both high frequency and low latency is important to ensure data is fetched as quickly as possible. 

* For modern systems using DDR5, a kit of RAM running at 6000 MT/s with a CAS latency (CL) of 30 or lower is recommended.  

* For older systems using DDR4, a kit of RAM running at 3600 MT/s with a CAS latency (CL) of 16 or lower is recommended.

**GPU**

At the time of writing this, Arma 3 has a performance bias towards NVIDIA GPUs, most likely due to different levels of support, or bugs (other Bohemia titles such as DayZ also exhibit this). So for instance, an AMD GPU which should be the same performance as a competing NVIDIA GPU would actually perform noticeably worse in GPU bound scenarios. Because of this I'd typically recommend an NVIDIA GPU if you want the absolute best performance, though it's not overly important as GPUs are not the bottleneck in the majority of gameplay scenarios.

# Parameters
Arma 3 supports launch parameters, which are configurable options that instruct the game to make use of various features and resources.

Only the recommended parameters for general use and optimal performance will be listed.

## Game launcher

### Where to find the parameters
* Open the Arma 3 launcher.
* Navigate to the "Parameters" tab found on the left side.
* At the top of the Parameters tab, click "All Parameters".

### Which parameters to set
* Show static background in menu
	* Enable this to prevent the game from loading the world for display in the background, unnecessarily consuming resources.
* Skip logos at startup
	* Enable this to disable the splash screens during start up, allowing you to get into the game faster.
* CPU count
	* Only recommended for use if you wish to *limit* how many cores the game has access to, otherwise when unchecked the game will use all available physical cores.
* Extra threads
	* Recommended to leave disabled, as the game already knows to do this when detecting how many cores the CPU has.
* Enable Hyper-Threading
	* Generally it is recommended to enable this for older systems with dated / less powerful CPUs. However, if you are on a modern CPU (Intel 12th gen / AMD Ryzen 7000 series or newer), you should typically disable this as it can worsen performance slightly. I suggest YAAB benchmarking with it enabled to see if it does provide a performance benefit on your system.  
	<sub>Please note that Enable Hyper-Threading will be ignored when using the CPU count parameter.</sub>
* Memory allocator (64-bit)
	* Enables the ability to switch between different memory allocators. mimalloc is *highly* recommended, otherwise disable unless experimenting with the already provided memory allocators.  
	<sub>mimalloc can be found within the **Memory allocators** section of this guide.</sub>
* Enabled Large-page Support
	* Recommended to enable for additional performance. This is an alternative memory management technique that uses larger memory blocks than the default (4KB) page size.  
	<sub>Please note that your Windows profile may not have the required privileges to use Large-pages even if this parameter is enabled, as they are kept in physical memory. Keeping data in physical memory requires the Lock Pages in Memory privilege discussed in the **Memory allocators** section of this guide.</sub>

## Steam launch options
Several supported parameters are not listed in the game launcher. Using Steam's launch options is the recommended method for enabling them, as some of the following parameters do not work from a parameters file.

### How to find and use Steam's launch options feature
* Right-click Arma 3 in your Steam library list, then click "Properties...".
* Under the General tab will be a "Launch Options" section with a text box below it, this is where you will write the parameters you want to use.
  
A list of all available parameters can be found [here](https://community.bistudio.com/wiki/Arma_3:_Startup_Parameters).

### Which parameters to set 
Optionally, you can copy the below prewritten parameters and paste them as is:  
```
-maxFileCacheSize=12288 -setThreadCharacteristics
```

* -setThreadCharacteristics
	* This registers the game's executable as a "Game" in Windows for additional performance.  
	<sup>Do **NOT** use this parameter if you are using Windows Server as your operating system, as it can freeze the entire operating system.</sup>
* -maxFileCacheSize
	* Sets the maximum filecache size (in MB) to be used for caching gamedata loaded from disk. Performance gains from this are likely negligible, however in certain situations it may help.  
	<sup>A value between 6144-12288 (6-12GB) should be more than enough, as the default is 2048 (2GB).</sup>

### Advanced parameters
I strongly recommend **not** using these parameters unless you already understand whether or not the architectural features of your CPU would benefit from them, as they have the potential to worsen your performance unless properly understood.  

**Please note that these parameters are currently only supported on Profiling build V32 or later.**

* -cpuAffinity
	* Tells the game which specific CPU cores to use, via a bitmask value.  
	<sup>This can be useful for CPUs with multiple CCDs, or P and E cores.</sup>
* -cpuMainThreadAffinity
	* Tells the game which specific CPU core to use for the game's main thread, via a bitmask value.

# Memory allocators
Memory allocators are components that manage how an application allocates and deallocates its data in RAM.

## Installing and using a memory allocator

### Installing a memory allocator

* Right-click Arma 3 in your Steam library list, highlight "Manage", then click "Browse local files". 
* Open the "Dll" folder found in the game's install directory, then place the memory allocator **.dll** file in there.

### Using a memory allocator
* Open the Arma 3 launcher.
* Navigate to the "Parameters" tab found on the left side.
* At the top of the Parameters tab, click "All Parameters".
* Scroll down to the "Advanced" section, and look for "Memory allocator (64-bit)".
* Tick the checkbox, which will now highlight a drop-down list next to it.
* From that drop-down list, select the name of the memory allocator you want to use.

	<sup>To revert back to the default memory allocator uncheck the "Memory allocator (64-bit)" checkbox, or follow these same steps, instead selecting "Intel TBB 4 allocator".</sup>

## mimalloc
mimalloc is a high performance memory allocator managed and maintained by Microsoft.  
John Gold King has created a fork of it which is designed for use with Arma 3.  

mimalloc is *highly* recommended, as performance gains can be very noticeable when Lock Pages in Memory is enabled.

### Downloading mimalloc
mimalloc is available at John Gold King's GitHub page, which can be found [here](https://github.com/GoldJohnKing/mimalloc/releases).

* To download the latest, or previous versions of mimalloc, look for the "Assets" section found at the bottom of each release, then download the **.dll** file contained within it. Downloading the Source code (zip) and (tar.gz) is not necessary.

## Taking full advantage of memory allocators
Arma 3 supports Large-pages, which is a feature that requires Lock Pages in Memory (LPIM) privileges. This prevents Windows from shifting Arma 3's application data into virtual memory. Enabling privileges for this can increase in-game performance, as virtual memory is significantly slower than physical memory (RAM).

### Enabling Lock Pages in Memory

Please note that a **Professional, Enterprise or Education** edition of Windows is required to access the LPIM feature.

* Press the keyboard shortcut Windows+R to open the Run command.
* Type "gpedit.msc" into the text box, then press Enter or click "OK" to open the Local Group Policy Editor.
* On the left side of the Local Group Policy Editor window, expand the hierarchy as follows.
* Computer Configuration -> Windows Settings -> Security Settings -> Local Policies. 
* Select the "User Rights Assignment" folder.
* This should open a large list of policies on the right side, scroll down until you find "Lock pages in memory".
* Double click the "Lock pages in memory" policy to open its properties.
* Under the Local Security Setting tab, click the "Add User or Group..." button.
* In the Select Users or Groups window, click the "Object Types..." button found in the top right corner.
* In the Object Types window, click the checkbox next to "Groups", then click OK.
* Find the text box at the bottom of the Select Users or Groups window.
* In this text box type "Administrators", then click the "Check Names" button.
* In this same text box type "Users", then click the "Check Names" button.
* Click the OK button to close the Select Users or Groups window.
* Click the OK button to close the Lock pages in memory Properties window.
* Exit out of the Local Group Policy Editor window.
* Restart your computer for the changes to take effect.

# Performance Profiling Build
Arma 3 has a publicly available branch utilizing performance binaries which can increase in-game performance.

**The profiling branch also serves as somewhat of a beta testing environment for changes and bug fixes that will later come to official updates for the game. As such from time to time you can experience bugs while using it, and should temporarily revert to stable or the previous profiling build if they affect your gameplay too much.**  
<sub>To get new profiling bugs fixed faster, you can report them in the **#perf_prof_branch** channel found in the official Arma [Discord server](https://discord.com/invite/arma).</sub>

## Accessing the profiling build
There are two methods for switching to the profiling build:
* **Steam (recommended):**
	* Close your game and launcher if they are running.
	* Right-click Arma 3 in your Steam library list, then click "Properties...".
	* On the left side of the newly opened window, click the "Betas" tab.
	* Click the drop-down list in the top right corner of the Betas tab, then select "profiling - Performance Profiling Build".
	* Steam will now perform a quick update, switching you over to the profiling build.  
	<sub>To revert back off of the profiling build follow these same steps, instead selecting "None" in the drop-down list.</sub>
	
* **Manually replacing the game executables:**
	* Visit the Bohemia forum thread found [here](https://forums.bohemia.net/forums/topic/160288-arma-3-stable-server-218-profiling-performance-binary-feedback/), then follow the steps to download and replace the game executables.  
	<sub>This method can also be used for reverting to previous profiling builds if bugs are occurring on the latest one.</sup>

# Graphical settings

In-game graphical settings won't be explained in too much depth, as the performance optimisation [wiki page](https://community.bistudio.com/wiki/Arma_3:_Performance_Optimisation) covers all of the settings and what impact they have on performance, as well as which components they are most taxing on.

### CPU intensive settings
* View distance.
	* Generally recommended to keep this at the minimum of what you require for your gameplay.
* Object view distance.
	*  Generally recommended to keep this at the minimum of what you require for your gameplay.
* Shadows and shadow distance.
	* Highly recommended to disable shadows, as their FPS reduction is often significant.  
 	**It is important to reduce shadow distance to the minimum value, as there is a bug which reduces FPS even if shadows are disabled.**  
	<sub>If you wish to keep shadows enabled, setting it to Standard or higher can sometimes yield more performance than Low, as this shifts more of the rendering computation from the CPU to the GPU.</sub>

### GPU intensive settings

<sup>Please note that some of these settings may appear to have little to no performance impact if the CPU or RAM are the primary bottleneck.</sup>
* Sampling.
	* Highly recommended to keep at 100%, setting this higher can result in lower FPS while barely improving visual quality.  
	<sub>Setting this lower than 100% can increase performance if the GPU is the major bottleneck, however it will often significantly degrade visual quality.</sub>
* Particles.
	* Generally recommended to set to Low, as various explosions and other particle effects can cause sudden FPS drops on higher settings.
* AO.
	* Generally recommended to disable, often incurs a slight but measurable performance impact when enabled.
* FSAA.
	* Recommended to set to 2x or 4x, going higher can greatly increase strain on the GPU.
* PPAA.
	* CMAA is highly recommended as it looks almost identical to SMAA, while being significantly less GPU intensive.

# Yet Another Arma Benchmark (YAAB)
YAAB is a widely used Arma 3 performance benchmarking scenario made by Sams. It is ideal for measuring differences in performance while experimenting with parameters, overclocking, graphical settings, memory allocators and profiling builds, etc.

## Downloading and using YAAB

### Downloading YAAB
Visit Sams' Steam Workshop page [here](https://steamcommunity.com/sharedfiles/filedetails/?id=375092418) to subscribe to the scenario.

Alternatively, you can navigate to the Arma 3 Workshop in your Steam client, then search for "YAAB" to subscribe to it through there.

### Using YAAB
* Start Arma 3.
* Once in the main menu, highlight the "Singleplayer" button up the top left, then click "Scenarios" in the dropdown menu.
* Double-click the "Yet Another Arma Benchmark" scenario found under the "Steam subscribed content" section.
* The scenario will now load and automatically begin playing through the benchmark scenes, providing performance results at the end.

## Some things to note
* Pressing "S" after the scenario has loaded will temporarily apply some standard preset graphics settings for the duration of the benchmark.
* While benchmarking it is generally recommended to keep your view distance relatively low (<2km), as higher view distances can quickly cause a CPU bottleneck. This can make it difficult to discern if your changes are having any effect on performance.
* To determine a proper average, it is recommended to run the benchmark 3-5 times.
* Results can vary slightly without changing anything. This is due to the AI heavy nature of the benchmark, and Arma 3 being sensitive to fluctuating CPU and RAM resources.
* For the most consistent results, it is recommended to close all other applications while benchmarking.
* Having mods enabled can affect benchmark results, even if they are not being used in the scenario.
* If you're using an AMD CPU with 3D V-Cache, benchmark results may show little to no difference when overclocking or tuning RAM. I suspect this happens because YAAB doesn't generate enough performance-sensitive data to fully saturate the cache to the point of needing to store excess in RAM.

# Frequently asked questions (FAQ)

### How much FPS can I expect to gain if I follow everything in this guide?
It's impossible to estimate a specific value or range, as there are literally *dozens* of factors which affect FPS in Arma 3, such as system hardware, missions, maps, mods, server player count, AI count, asset count and so forth. The only trend I've observed is that the more modern and high-end your hardware is, the more you gain from these tips.

### I did everything in this guide, but my FPS barely improved, why?
Without knowing specifics about your hardware, mods you use and missions you play, it's difficult to give an exact answer. However it's usually one of two things (or both) - either your hardware (or some part of it) is low-end or quite dated, or the missions you play / mods you use are very computationally demanding. Typically in these situations the only things you can do to notably improve FPS are upgrade your hardware or consider overclocking your existing hardware.

### Why am I getting a "Blocked loading of file" error when trying to use mimalloc?
Sometimes this happens for newly released versions of mimalloc, as there is a BattlEye whitelisting process to make sure the .dll is safe for use. Until the .dll has been whitelisted by BattlEye, it will prevent usage of it. This can sometimes take up to a few days, but it's usually pretty quick.
