======
vscode
======

c_cpp_properties.json
======================

.. code-block:: json
    :caption: c_cpp_properties.json

    {
        "configurations": [
            {
                "name": "Linux",
                "cStandard": "c11",
                "intelliSenseMode": "gcc-x64",
                "compileCommands": "${workspaceFolder}/compile_commands.json"
            }
        ],
        "version": 4
    }


.. code-block:: json
    :caption: launch.json

    {
      // Use IntelliSense to learn about possible attributes.
      // Hover to view descriptions of existing attributes.
      // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
      "version": "0.2.0",
      "configurations": [
        {
          "name": "(gdb) linux",
          "type": "cppdbg",
          "request": "launch",
          "preLaunchTask": "vm",
          // "preLaunchTask": "build then run vm",
          "program": "${workspaceRoot}/vmlinux",
          "miDebuggerServerAddress": "localhost:1234",
          "args": [],
          "stopAtEntry": true,
          "stopAtConnect": true,
          "cwd": "${workspaceFolder}",
          "environment": [],
          "externalConsole": false,
          "MIMode": "gdb",
          "miDebuggerArgs": "-n",
          "targetArchitecture": "x64",
          "setupCommands": [
            {
              "text": "set arch i386:x86-64:intel",
              "ignoreFailures": false
            },
            {
              "text": "dir .",
              "ignoreFailures": false
            },
            {
              "text": "add-auto-load-safe-path ./",
              "ignoreFailures": false
            },
            {
              "text": "-enable-pretty-printing",
              "ignoreFailures": true
            }
          ]
        }
      ]
    }


settings.json
======================
.. code-block:: json
    :caption: settings.json

    {
        "files.exclude": {
            "**/.*.*.cmd": true,
            "**/.*.d": true,
            "**/.*.S": true
        },
        "[c]": {
            "editor.detectIndentation": false,
            "editor.tabSize": 8,
            "editor.insertSpaces": false
        },
        "[json]": {
            "editor.tabSize": 4,
            "editor.insertSpaces": true
        }
    }

tasks.json
======================
.. code-block:: json
    :caption: tasks.json

    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "tasks": [
            {
                "label": "vm",
                "type": "shell",
                "command": "echo Starting QEMU&qemu-system-x86_64 -s -S -kernel ~/linux/arch/x86/boot/bzImage -initrd ~/initramfs.cpio.gz -nographic -append console=ttyS0 -serial mon:stdio -device e1000,netdev=net0 -netdev user,id=net0,hostfwd=tcp::5555-:22",
                "presentation": {
                    "echo": true,
                    "clear": true,
                    "group": "vm"
                },
                "isBackground": true,
                "problemMatcher": [
                    {
                        "pattern": [
                            {
                                "regexp": ".",
                                "file": 1,
                                "location": 2,
                                "message": 3
                            }
                        ],
                        "background": {
                            "activeOnStart": true,
                            "beginsPattern": "^(Starting QEMU)",
                            "endsPattern": "^(Starting QEMU)"
                        }
                    }
                ]
            },
            {
                "label": "build",
                "type": "shell",
                "command": "make",
                "args": [
                    "-j2"
                ],
                "group": {
                    "kind": "build",
                    "isDefault": true
                },
                "presentation": {
                    "echo": false,
                    "group": "build"
                }
            },
            {
                "label": "build then run vm",
                "dependsOrder": "sequence",
                "dependsOn": [
                    "build",
                    "vm"
                ],
            }
        ]
    }

