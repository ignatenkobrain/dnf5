configure_file(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/conf.py.in ${CMAKE_CURRENT_BINARY_DIR}/conf.py)


add_custom_target(doc)


add_subdirectory(sphinx)


add_custom_target(doxygen
    doxygen ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile
    COMMENT "Building doxygen xml files"
)


if(WITH_HTML)
    add_custom_target(doc-html
        sphinx-build -c ${CMAKE_CURRENT_BINARY_DIR} -b html
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_BINARY_DIR}/html
        COMMENT "Building html documentation"
    )
    add_dependencies(doc doc-html)
    add_dependencies(doc-html doxygen)

    #install(FILES ${HTML_FILES} )
endif()


if(WITH_MAN)
    add_custom_target(doc-man
        sphinx-build -c ${CMAKE_CURRENT_BINARY_DIR} -b man
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_BINARY_DIR}/man
        COMMENT "Building man documentation"
    )
    add_dependencies(doc doc-man)

    if(WITH_DNF5DAEMON_CLIENT)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5daemon-client.8 DESTINATION share/man/man8)
    endif()

    if(WITH_DNF5DAEMON_SERVER)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5daemon-server.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5daemon-dbus-api.8 DESTINATION share/man/man8)
    endif()

    if(WITH_DNF5)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-advisory.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-autoremove.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-clean.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-distro-sync.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-downgrade.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-download.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-environment.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-group.8 DESTINATION share/man/man8)
        # TODO(jkolarik): history is not ready yet
        # install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-history.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-install.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-leaves.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-makecache.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-mark.8 DESTINATION share/man/man8)
        # TODO(jkolarik): module is not ready yet
        # install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-module.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-reinstall.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-remove.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-repo.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-repoquery.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-search.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-swap.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-upgrade.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-comps.7 DESTINATION share/man/man7)
        # TODO(jkolarik): filtering is not ready yet
        # install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-filtering.7 DESTINATION share/man/man7)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-installroot.7 DESTINATION share/man/man7)
        # TODO(jkolarik): modularity is not ready yet
        # install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-modularity.7 DESTINATION share/man/man7)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-specs.7 DESTINATION share/man/man7)

        # plugin commands
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-builddep.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-copr.8 DESTINATION share/man/man8)
        install(FILES ${CMAKE_CURRENT_BINARY_DIR}/man/dnf5-repoclosure.8 DESTINATION share/man/man8)
    endif()

endif()
