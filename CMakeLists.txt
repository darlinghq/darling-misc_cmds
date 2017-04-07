project(misc_cmds)

add_compile_options(
	-nostdinc
)

add_definitions(
	-D__FBSDID=__RCSID
)

add_darling_executable(calendar
	calendar/calendar.c
	calendar/io.c
	calendar/day.c
	calendar/ostern.c
	calendar/paskha.c
	gen/calendar_vers.c
)
target_include_directories(calendar PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/calendar)
#add_darling_executable(leave leave/leave.c gen/leave_vers.c) currently having strange type issues
add_darling_executable(ncal ncal/ncal.c ncal/calendar.c ncal/easter.c gen/ncal_vers.c)
target_include_directories(ncal PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/ncal)
add_darling_executable(cal ncal/ncal.c ncal/calendar.c ncal/easter.c gen/ncal_vers.c)
target_include_directories(cal PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/ncal)
add_darling_executable(tsort tsort/tsort.c gen/tsort_vers.c)
add_darling_executable(units units/units.c gen/units_vers.c)
target_include_directories(units PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/units)

install(TARGETS calendar ncal cal tsort units #leave
	DESTINATION libexec/darling/usr/bin)

install(FILES
	calendar/calendar.1
	leave/leave.1
	ncal/ncal.1
	tsort/tsort.1
	units/units.1

	DESTINATION libexec/darling/usr/share/man/man1
)
InstallSymlink(ncal.1 libexec/darling/usr/share/man/man1/cal.1)

install(DIRECTORY calendar/calendars/ DESTINATION libexec/darling/usr/share/calendar)
# Not sure why the empty folders, but Apple does it as of misc_cmds-33, so we do it too
install(DIRECTORY DESTINATION libexec/darling/usr/share/calendar/de_DE.ISO8859-15)
install(DIRECTORY DESTINATION libexec/darling/usr/share/calendar/fr_FR.ISO8859-15)

install(FILES units/units.lib DESTINATION libexec/darling/usr/share/misc)
