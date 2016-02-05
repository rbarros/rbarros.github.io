---
layout: post
title:  "Iniciar Vagrant no Windows com bat"
date:   2016-01-31 21:57:00
categories: windows
tags: [vagrant,windows,bat]
---
Iniciar a VM do vagrant através de um arquivo `.bat`.

{% highlight bat %}
@echo off

CD /d \path\dir\vagrant

FOR /f "tokens=2 delims= " %%a in ('vagrant status ^| find /i "default"') do (
    SET STATE=%%a
)

IF %STATE% equ "running" (
    @echo Vagrant VM is running...
) ELSE (
    IF %STATE% equ "saved" (
        @echo Starting Vagrant VM from powered down state...
        vagrant up
    ) ELSE (
        IF %STATE% equ "poweroff" (
            @echo Starting Vagrant VM from powered down state...
            vagrant up
        ) else (
            @echo Resuming Vagrant VM from saved state...
            vagrant resume
        )
    )
)

IF errorlevel 1 (
  @echo FAILURE! Vagrant VM unresponsive...
)
{% endhighlight %}
