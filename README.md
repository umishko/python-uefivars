# uefivars

This is a set of Python modules and a helper application "uefivars" to
introspect and modify UEFI variable stores.

## Why do I need this?

UEFI variable stores are typically opaque to users. You access them using
UEFI runtime services as function calls. However, the data is then stored
in a binary data format. When running virtual machines or extracting UEFI
variable stores directly from Flash storage, you can receive and write that
binary data and thus modify variables directly.

This is useful in situations where you have incorrect UEFI variable data
and need to modify variables without runtime service access. It can also
be useful to analyze and introspect the variable store and check what data
is stored inside.

## How do I use it?

You can convert a variable store into human readable format by setting the
output type to json. This will show you all variables that are currently
present in the variable store.

```console
$ uefivars -i edk2 -o json -I OVMF_VARS.secboot.fd
[
    {
        "name": "certdb",
        "data": "04 00 00 00",
        "guid": "6e e5 be d9 dc 75 d9 49 b4 d7 b5 34 21 0f 63 7a",
        "attr": 39,
        "timestamp": "00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00",
        "digest": ""
    },
    [...]
]
```

In addition, you can convert from the human readable json representation back
into edk2 format:

```console
$ uefivars -i json -o edk2 -I vars.json -O OVMF_VARS.fd
```

You can also use the tool to convert between the AWS EC2 uefi-data format
and edk2 to import and export UEFI variable stores between an EC2 instance
and QEMU:

```console
$ uefivars -i edk2 -o aws -I OVMF_VARS.fd -O uefi-data.aws
```

```console
$ uefivars -i aws -o edk2 -I uefi-data.aws -O OVMF_VARS.fd
```

## What formats are supported?

This package currently supports the aws, edk2 and json file formats.
