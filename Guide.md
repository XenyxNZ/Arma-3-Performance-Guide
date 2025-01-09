# Understanding hardware performance in Arma 3

### Why it's not great

Arma 3 is running on Bohemia's Real Virtuality engine which is heavily constrained by the CPU and RAM due to the lack of multithreading in critical areas. Contrary to popular belief, the game does actually support multithreading, and has since release - just not in some of the critical aspects of the game's engine, which results in severe performance bottlenecks. This is why even the most modern, high-end systems can still struggle to run Arma 3 at high frame rates.

### How it can be better

Arma 3 is very responsive to CPU single core performance, cache performance & size, as well as RAM bandwidth and latency. Nowadays it's possible to get very good out-of-the-box frame rates using a CPU which utilizes AMD's 3D cache technology - these CPUs have significantly more on-board memory (cache), which the game will use to store frequently accessed data. CPU cache has significantly higher bandwidths than RAM, and the access latency is several times lower. Traditional overclocking techniques can still greatly improve performance, especially on Intel based systems (or AMD systems without 3D cache). Overclocking the CPU and RAM on systems utilizing an AMD CPU with 3D cache can still reap performance benefits, however it's not necessary for good performance, as your baseline FPS is considerably higher.

## Choice of hardware

**CPU**

As mentioned above, an AMD CPU with 3D cache is *highly* recommended for your next PC build, or upgrade. These CPUs not only improve performance in Arma 3, but also almost every other game. Even if you are on an older AM4 socket system, you can still reap the benefits of this technology with a 5800X3D, 5700X3D or 5600X3D.

**RAM**

* For modern systems using DDR5, a kit of RAM running at 6000 Mt/s with a CAS (CL) latency of 30 or lower is recommended.

Intel systems are able to utilize much higher memory speeds than 6000 Mt/s, however as of writing this, the ability to stably run speeds beyond ~6800 MT/s is extremely dependent on the motherboard & quality of the CPU's integrated memory controller (IMC). I won't go into any deeper of an explanation than that, as it would require a whole page solely dedicated to the explanation of various motherboard componentry, RAM ICs & CPU IMCs.

* For older systems using DDR4, a kit of RAM running at 3600 MT/s with a CAS (CL) latency of 16 or lower is recommended.

# Game Launcher
The game launcher has a parameters section where you can configure which system features & resources the game can make use of.

### Getting to the parameters

* Open the Arma 3 launcher.
* Click on "Parameters" found on the left side.
* Click on "All Parameters" found at the top of the parameters tab.

### Setting parameters
Only the recommended features for general use & optimal performance will be listed.

* Show static background in menu
	* Enable this to prevent the game from loading the world for display in the background, unnecessarily consuming resources.
* Skip logos at startup
	* Enable this to disable the splash screens during start up, allowing you to get into the game faster.
* CPU count
	* Only recommended for use if you wish to *limit* how many cores the game has access to.
* Extra threads
	* Enable this and the 3 extra checkboxes next to it. Hands off tasks to other threads to help decongest workloads on the main thread.
* Memory allocator (64-bit)
	* Enables the ability to switch between different memory allocators. mimalloc is *highly* recommended for this, otherwise disable unless experimenting with the already provided memory allocators.
* Enabled Large-page Support
	* Recommended to enable for additional performance. This is an alternative memory management technique that uses larger memory blocks than the default (4KB) page size.

# Memory Allocators
Memory allocators are application components that manage how it allocates and deallocates its data in RAM.

## Installing & using a memory allocator

### Installing

* Right-click Arma 3 in your Steam library list, highlight "Manage" then click "Browse local files". 
* Open the "Dll" folder found in the game's install directory, then place the memory allocator **.dll** file in there.

### Using a memory allocator
* Open the Arma 3 launcher.
* Navigate to the "Parameters" tab found on the left side.
* At the top of the Parameters tab, click "All Parameters".
* Scroll down to the "Advanced" section, and look for "Memory allocator (64-bit)".
* Tick the checkbox, which will now highlight a drop-down list next to it.
* From that drop-down list, select the name of the memory allocator you want to use.
To revert back to the default memory allocator follow these same steps, instead selecting "Intel TBB 4 allocator".

## mimalloc
mimalloc is a high performance memory allocator managed & maintained by Microsoft.
John Gold King has created a fork of it which is designed to be compatible with Arma 3.

### Downloading mimalloc
mimalloc is available at John Gold King's GitHub page, which can be found [here](https://github.com/GoldJohnKing/mimalloc/releases).
* To download the latest, or previous versions of mimalloc, look for the "Assets" section found at the bottom of each release, then download the **.dll** file contained within it. Downloading the Source code (zip) and (tar.gz) is not necessary.

### Taking full advantage of mimalloc
John Gold King's mimalloc supports the "Lock Pages in Memory (LPIM)" feature which prevents Windows from shifting Arma 3's application data into virtual memory. This can increase in-game performance, as virtual memory is considerably slower than physical memory (RAM).

**Enabling Lock Pages in Memory:**
Please note that a **Professional** edition of Windows is required to access the LPIM feature.
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

**At the time of writing this, Dedmen is currently testing multiple mulithreading improvements on the profiling build, which can significantly increase in-game performance.**

**The profiling branch also serves as somewhat of a beta testing environment for changes and bug fixes that will later come to official updates for the game. As such from time to time you can experience bugs while using it, and should temporarily revert to stable or the previous profiling build if they affect your gameplay too much.**

To get new profiling bugs fixed faster, you can report them in the **#perf_prof_branch** channel found in the official Arma [Discord server](https://discord.com/invite/arma).

## Accessing the profiling build
There are two methods for switching to the profiling build:
* **Steam (recommended):**
	* Close your game and launcher if they are running.
	* Right-click Arma 3 in your Steam library list, then click "Properties...".
	* On the left side of the newly opened window, click the "Betas" tab.
	* Click the drop-down list in the top right corner of the Betas tab, then select "profiling - Performance Profiling Build".
	* Steam will now perform a quick update, switching you over to the profiling build.
	To revert back off of the profiling build follow these same steps, instead selecting "None" in the drop-down list.
	
* **Manually replacing the game executables:**
	This method can also be used for reverting to previous profiling builds if bugs are occurring on the latest one.
	* Visit the Bohemia forum thread found [here](https://forums.bohemia.net/forums/topic/160288-arma-3-stable-server-218-profiling-performance-binary-feedback/), then follow the steps to download and replace the game executables.

# Graphical Settings

In-game graphics settings won't be explained in too much depth, as the performance optimisation [wiki page](https://community.bistudio.com/wiki/Arma_3:_Performance_Optimisation) covers all of the settings and what impact they have on performance, as well as which components they are most taxing on.

**CPU taxing settings which contribute to the most FPS loss are:**
* View distance.
	* Generally recommended to keep this at the minimum of what you require for your gameplay.
* Object view distance.
	*  Generally recommended to keep this at the minimum of what you require for your gameplay.
* Shadows & shadow distance (when on low/med settings).
	* Highly recommended to disable shadows, as their FPS reduction is often significant.
	If you wish to keep shadows enabled, counterintuitively setting them to high/ultra can sometimes yield more performance than low/medium, as this switches most of the shadow rendering computation to the GPU.

**GPU taxing settings which contribute to the most FPS loss are:**
Please note that some of these settings may appear to have little to no performance impact if your CPU or RAM are the primary bottleneck.
* Sampling.
	* Generally recommended to keep at 100%, setting this higher can result in worse FPS while barely improving visual quality.
* Shadows & shadow distance (when on high/ultra settings).
	* Highly recommended to disable shadows, as their FPS reduction is often significant.
* Particles.
	* Generally recommended to set to Low, as various explosions and other particle effects can cause sudden FPS drops on higher settings.
* AO.
	* Generally recommended to disable, often incurs a slight but measurable performance impact when enabled.
* FSAA.
	* Generally recommended to set to 2x or 4x, going higher can put a lot of strain on your GPU.
* PPAA.
	* CMAA is highly recommended as it looks almost identical to SMAA, while being significantly less GPU intensive.
