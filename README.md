# Understanding performance in Arma 3

### Why it's not great

Arma 3 is built on Bohemia's Real Virtuality engine, and contrary to popular belief, it does utilize multithreading for many things. However, it still suffers from severe performance bottlenecks in critical aspects due to limitations in its design. This is why even the most modern, high-end systems can still struggle to run Arma 3 at high frame rates 10+ years after its release.


## Choice of hardware

### CPU

Arma 3 is very responsive to CPU core performance, as well as cache size. Nowadays it's possible to get very good out-of-the-box frame rates using a CPU which utilizes AMD's 3D V-Cache technology - these CPUs have significantly more on-board memory (cache), which is used to store frequently accessed data. The more cache memory a CPU has, the more performance-sensitive data can be stored in it, as opposed to fetching it from RAM, which is significantly slower by comparison. 

As mentioned above, an AMD CPU with 3D V-Cache is *highly* recommended for your next PC build, or upgrade. These CPUs not only greatly improve performance in Arma 3, but also almost every other game. Even if you are on an older AM4 socket system, you can still reap the benefits of this technology with a 5800X3D, 5700X3D or 5600X3D.

### RAM

Just like CPUs, Arma 3 is very responsive to RAM performance. Only a comparatively small amount of data can be stored in CPU cache, so having a kit of RAM which is both high frequency and low latency is important to ensure data is fetched as quickly as possible. Note that Arma 3 tends to benefit more from lower latency than from raw bandwidth, which is worth keeping in mind when comparing kits.

Before anything else, make sure your RAM's XMP (Intel) or EXPO/DOCP (AMD) profile is enabled in your motherboard's BIOS. RAM does not run at its advertised speed out of the box - without a profile enabled, a 6000 MT/s kit will typically run at a much slower default speed (e.g. 4800 MT/s with very loose timings), and this is one of the most common and easily fixed performance losses in any system.

AMD (AM5)

* A kit running at 6000 MT/s with a CAS latency (CL) of 30 or lower is recommended. This is because 6000 MT/s is generally the highest speed at which the memory controller and Infinity Fabric can run in their optimal 1:1 ratio - higher frequencies apply a desynchronized ratio, where the resulting added latency usually cancels out the bandwidth gains.

* If you're using a CPU with 3D V-Cache, RAM speed and timings matter significantly less, as the large cache absorbs much of the traffic that would otherwise hit RAM. A standard 5600-6000 CL32-36 kit gets you 95% of the way there; spending extra on a low-latency kit will yield marginal performance gains.

Intel (LGA1851)

* Intel's officially supported memory speed increases with each generation - the original Core Ultra 200S series supports up to 6400 MT/s, while the newer 200S Plus refresh supports up to 7200 MT/s. A kit at or near your CPU's official supported maximum speed with the lowest CL you can find is recommended.

* Speeds well beyond official spec (8000+ MT/s) are achievable on these platforms, particularly with CUDIMM memory. However, be aware that at high frequencies the memory controller runs at a divided ratio (Gear 2/Gear 4), so real-world latency does not improve anywhere near as much as the bandwidth figures suggest. These kits also carry a heavy price premium, and a potential stability tuning burden that is hard to justify for the modest FPS difference if you're unfamiliar with RAM overclocking/tuning.

Intel (LGA1700 — 12th/13th/14th gen)

* Official DDR5 support is 4800 MT/s on 12th gen and 5600 MT/s on 13th/14th gen, however in practice these CPUs can typically run faster kits without issue. For 13th/14th gen, a 6600–6800 MT/s kit with the lowest CL you can find is recommended; 12th gen memory controllers are weaker, so 6000–6400 MT/s is more achievable without running into stability issues.

* LGA1700 also supports DDR4, depending on the motherboard. If you're on a DDR4 board, the DDR4 recommendations below apply.

Older systems (DDR4)

* A kit running at 3600 MT/s with a CAS latency (CL) of 16 or lower is recommended.
* On AM4 Ryzen systems, 3600 MT/s is the practical ceiling for keeping the Infinity Fabric in its 1:1 ratio (FCLK 1800 MHz) - the same principle as AM5 above.

**Note:** 

Maximum stable RAM speed depends on several factors including CPU memory controller, motherboard, RAM stick density/ranks, and total RAM sticks installed. 

**If you are planning to run high density kits (e.g. 2x32GB or 2x48GB), or four sticks instead of two, disregard the aforementioned DDR5 speed recommendations above 6000 MT/s, as these configurations put much more strain on the CPU memory controller, limiting how fast they can run without causing instability.**

### GPU

Arma 3 favors NVIDIA GPUs due to lower driver overhead in DX11 games. NVIDIA's DX11 driver distributes its command submission work across multiple CPU threads, while AMD's DX11 driver places a heavier load on a single CPU thread. In a game as CPU-limited as Arma 3, that extra driver overhead compounds the engine's existing main-thread bottleneck, meaning an AMD GPU that should perform on par with its NVIDIA equivalent can deliver noticeably lower FPS in the CPU-bound scenarios that make up the majority of Arma 3 gameplay.

For this reason I'd typically recommend an NVIDIA GPU if you want the absolute best performance. That said, if you're genuinely GPU-bound due to running very high resolutions and/or graphical settings, this driver overhead mostly disappears.

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
	* Enable this to disable the splash screens during start up, allowing you to get into the game slightly faster.
* CPU count
	* Only recommended for use if you wish to *limit* how many cores the game has access to, otherwise when unchecked the game will use all available physical cores.
* Extra threads
	* Recommended to leave disabled, as the game already knows to do this when detecting how many cores the CPU has.
* Enable Hyper-Threading
	* Generally it is recommended to enable this. However if you're using a modern CPU with 8 or more cores, disabling Hyper-Threading can sometimes yield slightly more performance. Try YAAB benchmarking with it disabled to see if it does provide a performance benefit on your system.  
	<sub>Please note that Enable Hyper-Threading will be ignored when using the CPU count parameter.</sub>
* Memory allocator (64-bit)
	* Enables the ability to switch between different memory allocators. mimalloc is *highly* recommended, otherwise leave disabled unless experimenting with the already provided memory allocators.  
	<sub>mimalloc can be found within the **Memory allocators** section of this guide.</sub>
* Enabled Large-page Support
	* Recommended to enable for additional performance. This enables the use of larger (2MB) memory pages, increasing memory-management efficiency.  
	<sub>Please note that by default your Windows profile won't have the required privileges to use Large-pages even if you enable this parameter. For programs to use Large-pages, Lock Pages in Memory needs to be configured in the Group Policy Editor, which is discussed in the **Memory allocators** section of this guide.</sub>
* Command line
	* Recommended method for setting the parameters discussed below.

## Command line
Several supported parameters are not listed in the game launcher. Using the launcher's command line parameter is the recommended method for passing them to the game.

A list of all available parameters can be found [here](https://community.bistudio.com/wiki/Arma_3:_Startup_Parameters).

### Which parameters to set 

* -setThreadCharacteristics
	* This registers the game's executable as a "Game" in Windows for additional performance.  
	<sup>Do **NOT** use this parameter if you are using Windows Server as your operating system, as it can freeze the entire operating system.</sup>
* -maxFileCacheSize
	* Sets the maximum filecache size (in MB) to be used for caching gamedata loaded from disk. Performance gains from this are likely negligible, however in certain situations it may help.  
	<sup>A value between 6144-12288 (6-12GB) should be more than enough, as the default size is 2048 (2GB).</sup>

Optionally, you can copy the below prewritten parameters and paste them into the Command line parameter as is:  
```
-setThreadCharacteristics -maxFileCacheSize=12288
```

### Advanced parameters
I strongly recommend **not** using these parameters unless you already understand whether or not the architectural features of your CPU can benefit from them, as they have the potential to worsen performance unless properly understood.  

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
Gold John King has created a fork of it which is designed for use with Arma 3.  

mimalloc is *highly* recommended, as performance gains can be very noticeable when Lock Pages in Memory and Large-pages are enabled.

### Downloading mimalloc
mimalloc is available at Gold John King's GitHub page, which can be found [here](https://github.com/GoldJohnKing/mimalloc/releases).

* To download the latest, or previous versions of mimalloc, look for the "Assets" section found at the bottom of each release, then download the **.dll** file contained within it. **Downloading the Source code (zip) and (tar.gz) is not necessary.**

## Taking full advantage of memory allocators
To get the most out of memory allocators, some Windows policy configuration is required.

### Enabling Lock Pages in Memory
As mentioned in the parameters section, to make use of Large-pages you must configure Lock Pages in Memory.  

Lock Pages in Memory can provide performance benefits of its own, as it prevents Windows from shifting Arma 3's data into the pagefile. Keeping data out of the pagefile can reduce stutters, since it never needs to be reloaded from disk.

**Please note that a Professional, Enterprise or Education edition of Windows is required to access the Lock Pages in Memory feature.**

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
 	**It's important to reduce shadow distance to the minimum value, as there is a bug which reduces FPS even if shadows are disabled.**  
	<sub>If you wish to keep shadows enabled, setting it to Standard or higher can sometimes yield more performance than Low, as this shifts more of the rendering computation from the CPU to the GPU.</sub>

### GPU intensive settings

<sup>Please note that some of these settings may appear to have little to no performance impact if the CPU or RAM are the primary bottleneck.</sup>
* Particles.
	* Generally recommended to set to Low, as various explosions and other particle effects can cause significant dips in frame rate on higher settings.
* AO.
	* Generally recommended to disable, often incurs a slight but measurable performance impact when enabled.
* FSAA.
	* Recommended to set to 2x or 4x, going higher will greatly increase strain on the GPU while only looking marginally better.
* PPAA.
	* CMAA is highly recommended as it looks almost identical to SMAA, while being significantly less GPU intensive.

# Yet Another Arma Benchmark (YAAB)
YAAB is a widely used Arma 3 performance benchmarking scenario made by Sams. It's ideal for measuring differences in performance while experimenting with parameters, overclocking, graphical settings, memory allocators and profiling builds, etc.

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
It's impossible to estimate a specific value or range, as there are *dozens* of factors which affect FPS in Arma 3, such as system hardware, missions, maps, mods, server player count, AI count, asset count and so forth. The only trend I've observed is the more modern and high-end your hardware is, the more you gain from these tips.

### I did everything in this guide, but my FPS barely improved, why?
Without knowing specifics about your hardware, mods you use and missions you play, it's difficult to give an exact answer. However it's usually one of two things (or both) - either your hardware (or some part of it) is low-end or dated, or the missions you play / mods you use are very computationally demanding. Typically in these situations the only things you can do to notably improve FPS are upgrade your hardware or consider overclocking your existing hardware.

### Why am I getting a "Blocked loading of file" error when trying to use mimalloc?
Sometimes this happens for newly released versions of mimalloc, as there is a BattlEye whitelisting process to make sure the .dll is safe for use. Until the .dll has been whitelisted by BattlEye, it will prevent usage of it. This can sometimes take up to a few days, but it's usually pretty quick.




