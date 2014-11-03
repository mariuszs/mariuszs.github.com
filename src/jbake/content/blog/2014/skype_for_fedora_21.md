title=Skype for Fedora 21
date=2014-11-03
type=post
tags=blog, skype, fedora
status=published
~~~~~~


Prepare rpm building environment
---
Install the rpm building environment as shown here: [How to create an RPM package](http://fedoraproject.org/wiki/How_to_create_an_RPM_package#SPEC_example)

    sudo yum install @development-tools
    sudo yum install fedora-packager
    sudo usermod -a -G mock $USER
    rpmdev-setuptree

Download Skype
----

Download Dynamic Skype from http://www.skype.com/en/download-skype/skype-for-computer/

    wget --trust-server-names -P ~/rpmbuild/SOURCES/ http://www.skype.com/go/getskype-linux-dynamic

This tutorial is for `skype-4.3.0.37.tar.bz2`.


Create desktop application shortcut for Skype
----

In the `~/rpmbuild/SOURCES` make the file `skype.desktop` with this content:

    [Desktop Entry]
    Name=Skype
    Comment=Skype Internet Telephony
    Exec=env PULSE_LATENCY_MSEC=60 skype %U
    Icon=skype.png
    Terminal=false
    Type=Application
    Encoding=UTF-8
    Categories=Network;Application;
    MimeType=x-scheme-handler/skype;
    X-KDE-Protocols=skype

Create Skype RPM Specification
----

In `~/rpmbuild/SPECS` create file `skype.spec` with this content:

    # skype.spec for skype-4.3.0.37 (x32)
    #
    # Build with: rpmbuild -ba --target=i686 skype.spec
    #
    # Original skype.spec for 4.2.0.11
    # https://github.com/mopsfelder/skype-rpm/blob/master/skype.spec

    #%global debug_package %{nil}

    Name: skype
    Version: 4.3.0.37
    Release: 1%{?dist}
    Summary: Skype is a free Internet telephony from Microsoft

    License: Commercial
    URL: http://www.skype.com/products/skype/linux/
    Source0: %{name}-%{version}.tar.bz2

    Requires: alsa-lib
    Requires: glibc
    Requires: libgcc
    Requires: libpng
    Requires: libX11
    Requires: libXext
    Requires: libXScrnSaver
    Requires: libXv
    Requires: libstdc++
    Requires: qt >= 4.6
    Requires: qt-x11
    Requires: qtwebkit

    #ExcludeArch: x86_64
    #AutoReqProv: no

    %description
    Skype is a free Internet telephony from Microsoft.

    %prep
    %setup -q

    %build

    %install
    rm -rf %{buildroot}
    %{__mkdir_p} %{buildroot}
    %{__mkdir_p} %{buildroot}%{_bindir}
    %{__mkdir_p} %{buildroot}%{_datadir}/applications
    %{__mkdir_p} %{buildroot}%{_datadir}/%{name}
    %{__mkdir_p} %{buildroot}%{_sysconfdir}/dbus-1/system.d

    %{__cp} %{name} %{buildroot}%{_bindir}
    %{__cp} %{name}.conf %{buildroot}%{_sysconfdir}/dbus-1/system.d
    %{__cp} %{name}.desktop %{buildroot}%{_datadir}/applications

    # Resources
    for DIR in avatars lang sounds; do
        %{__cp} -r $DIR %{buildroot}%{_datadir}/%{name}
    done

    # Icons
    for SIZE in 16 24 32 48 64 96 128 256; do
        %{__mkdir_p} %{buildroot}%{_datadir}/icons/hicolor/${SIZE}x ${SIZE}/apps
        %{__cp} icons/SkypeBlue_${SIZE}x${SIZE}.png \
            %{buildroot}%{_datadir}/icons/hicolor/${SIZE}x ${SIZE}/apps/%{name}.png
    done

    %files
    %defattr(0755,root,root,0755)
    %{_bindir}/%{name}
    %defattr(0644,root,root,0755)
    %{_sysconfdir}/dbus-1/system.d/%{name}.conf
    %{_datadir}/applications/%{name}.desktop
    %{_datadir}/%{name}/avatars/*
    %{_datadir}/%{name}/lang/*
    %{_datadir}/%{name}/sounds/*
    %{_datadir}/icons/*
    %dir %{_datadir}/%{name}/avatars
    %dir %{_datadir}/%{name}/lang
    %dir %{_datadir}/%{name}/sounds
    %dir %{_datadir}/%{name}
    %doc LICENSE README third-party_attributions.txt

    %changelog
    * Fri Jun 20 2014 Cristian Sava <cristis53 at gmail.com> 4.3.0.37-1
    - Version 4.3.0.37 (i686) for Fedora 20

    * Thu Feb 26 2014 Cristian Sava <cristis53 at gmail.com> 4.2.0.13-1
    - Version 4.2.0.13 (i686) for Fedora 20

    * Wed Jul 31 2013 Murilo Opsfelder Araujo <mopsfelder at gmail.com>
    4.2.0.11-1
    - Initial version

Build skype RPM
----

    rpmbuild -ba --target=i686 ~/rpmbuild/SPECS/skype.spec

You will get `skype-4.3.0.37-1.fc21.i686.rpm` in `~/rpmbuild/RPMS/i686` directory

Installation
----

    sudo yum localinstall ~/rpmbuild/RPMS/i686/skype-4.3.0.37-1.fc21.i686.rpm



Authors
---

This tutorial is based on excellent instruction from fedora mailing list: [The new skype-4.3.0.37 is out. Build your own rpm for Fedora 20](https://lists.fedoraproject.org/pipermail/users/2014-June/450773.html).
All the glory goes to *Cristian Sava* and [*Murilo Opsfelder Araujo*](https://github.com/mopsfelder/skype-rpm/).

