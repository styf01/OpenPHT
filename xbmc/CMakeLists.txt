
add_subdirectory(cores)
add_subdirectory(addons)
add_subdirectory(cdrip)
add_subdirectory(dbwrappers)
add_subdirectory(dialogs)
add_subdirectory(filesystem)
add_subdirectory(guilib)
add_subdirectory(input)
add_subdirectory(interfaces)
add_subdirectory(music)
add_subdirectory(network)
add_subdirectory(peripherals)
add_subdirectory(pictures)
add_subdirectory(playlists)
add_subdirectory(powermanagement)
add_subdirectory(programs)
add_subdirectory(rendering)
add_subdirectory(settings)
add_subdirectory(storage)
add_subdirectory(threads)
add_subdirectory(utils)
add_subdirectory(video)
add_subdirectory(windowing)
add_subdirectory(windows)

if(APPLE)
	add_subdirectory(osx)
endif(APPLE)
if(UNIX)
	add_subdirectory(linux)
endif(UNIX)

aux_source_directory(. xbmc_SRCS)

set(osx_frameworks
	AudioToolbox
	AudioUnit
	Cocoa
	CoreAudio
	CoreServices
	Foundation
	OpenGL
	AppKit
	ApplicationServices
	IOKit
	QuickTime
	Carbon
	DiskArbitration
	QuartzCore
	SystemConfiguration
)

set(external_libs
	z
#	bz2
	xml2
	iconv
#	ncurses
	freetype
	SDL
	SDL_mixer
	SDL_image
	microhttpd
	tinyXML
#	boost_thread
#	boost_date_time
	GLEW
	sqlite3
	yajl
	ssh
	ssh2
	cdio
	pcre
	pcrecpp
	jpeg
	lzo2
	samplerate
	fribidi
)
set(system_libs
	stdc++
)

add_executable(XBMC ${xbmc_SRCS})
target_link_libraries(XBMC
	utils
	dllloader
	exports
	addons
	cdrip
	dbwrappers
	dialogs
	filesystem
	MusicDatabaseDirectory
	VideoDatabaseDirectory
	guilib
	input
	interfaces
	httpapi
	jsonrpc
	info
	music
	music_infoscanner
	music_tags
	music_dialogs
	music_karaoke
	music_windows
	network
	network_scrobbler
	network_osx
	network_linux
	osx
	peripherals
	peripherals_bus
	peripherals_devices
	peripherals_dialogs
	pictures
	powermanagement
	playlists
	programs
	rendering
	settings
	storage
	threads
	threads_platform
	video
	video_dialogs
	video_windows
	windowing
	windows
	cores
	audiorenderers
	dvdplayer
	dvdplayer_dvdcodecs
	dvdplayer_dvdcodecs_audio
	dvdplayer_dvdcodecs_video
	dvdplayer_dvdcodecs_overlay
	dvdplayer_dvddemuxers
	dvdplayer_dvdinputstreams
	dvdplayer_dvdsubtitles
	externalplayer
	paplayer
	playercorefactory
	videorenderers
	videorenderers_shaders
	xbmc_linux
	lib_squish
	lib_apetag
	lib_hts
)

foreach(lib ${external_libs})
	find_library(${lib}_LIBRARY ${lib} ${dependdir}/lib NO_DEFAULT_PATH)
	get_property(l VARIABLE PROPERTY ${lib}_LIBRARY)
	target_link_libraries(XBMC ${l})
endforeach()

foreach(lib ${system_libs})
	find_library(${lib}_LIBRARY ${lib})
	get_property(l VARIABLE PROPERTY ${lib}_LIBRARY)
	target_link_libraries(XBMC ${l})
endforeach()


foreach(framework ${osx_frameworks})
	find_library(${framework}_LIBRARY ${framework})
	get_property(f VARIABLE PROPERTY ${framework}_LIBRARY)
	mark_as_advanced(${f})
	target_link_libraries(XBMC ${f})
endforeach()