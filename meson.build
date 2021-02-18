project('jacktrip', 'cpp', version: '1.2',
		default_options: ['cpp_std=c++11'])
qt5 = import('qt5')
cmake = import('cmake')

defines = []
if host_machine.system() == 'linux'
	defines += '-D__LINUX__'
elif host_machine.system() == 'darwin'
	defines += '-D__MAC_OSX__'
elif host_machine.system() == 'windows'
	defines += '-D__WIN_32__'
	defines += '-D_WIN32_WINNT=0x0600'
endif


qt5_dep = dependency('qt5', modules: ['Core', 'Network'])
jack_dep = dependency('jack', required: false)
if not jack_dep.found()
	defines += '-D__NO_JACK__'
endif

defines += '-D__RT_AUDIO__'
rtaudio_dep = dependency('rtaudio', required: false)
if not rtaudio_dep.found()
rtaudio = cmake.subproject('rtaudio')
rtaudio_dep = rtaudio.dependency('rtaudio')
endif

thread_dep = dependency('threads')

moc_h = ['src/DataProtocol.h',
	'src/JackTrip.h',
	'src/JackTripWorker.h',
	'src/JackTripWorkerMessages.h',
	'src/NetKS.h',
	'src/PacketHeader.h',
	'src/Settings.h',
	'src/UdpDataProtocol.h',
	'src/UdpHubListener.h']
moc_files = qt5.preprocess(moc_headers : moc_h)

src = ['src/DataProtocol.cpp',
	'src/JackTrip.cpp',
	'src/AudioTester.cpp',
	'src/jacktrip_globals.cpp',
	'src/jacktrip_main.cpp',
	'src/JackTripThread.cpp',
	'src/JackTripWorker.cpp',
	'src/LoopBack.cpp',
	'src/PacketHeader.cpp',
	'src/ProcessPlugin.cpp',
	'src/RingBuffer.cpp',
	'src/JitterBuffer.cpp',
	'src/Settings.cpp',
	'src/UdpDataProtocol.cpp',
	'src/UdpHubListener.cpp',
	'src/AudioInterface.cpp',
	'src/RtAudioInterface.cpp',
	'src/Compressor.cpp',
	'src/Limiter.cpp',
	'src/Reverb.cpp']

if jack_dep.found()
src += 	['src/JackAudioInterface.cpp',
	'src/JMess.cpp']
endif

executable('jacktrip', src, moc_files, dependencies: [qt5_dep, jack_dep, rtaudio_dep, thread_dep], cpp_args: defines, install: true )
