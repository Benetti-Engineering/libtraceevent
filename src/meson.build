# SPDX-License-Identifier: LGPL-2.1
#
# Copyright (c) 2023 Daniel Wagner, SUSE LLC

sources= [
   'event-parse-api.c',
   'event-parse.c',
   'event-plugin.c',
   'kbuffer-parse.c',
   'parse-filter.c',
   'parse-utils.c',
   'tep_strerror.c',
   'trace-seq.c',
   'trace-btf.c',
]

cc = meson.get_compiler('c')
dl_dep = cc.find_library('dl')

btf_enum64_test_code = '''
#include <linux/btf.h>
int main(void) {
    struct btf_enum64 e = {};

    return e.name_off + e.val_lo32 + e.val_hi32 +
           BTF_KIND_ENUM64;
}
'''

if not cc.compiles(btf_enum64_test_code, name : 'BTF_KIND_ENUM64 check')
    add_project_arguments('-DNO_BTF', language : 'c')
endif

libtraceevent = library(
    'traceevent',
    sources,
    version: library_version,
    dependencies: [dl_dep],
    include_directories: [incdir],
    install: true)

pkg = import('pkgconfig')
pkg.generate(
    libtraceevent,
    subdirs: 'traceevent',
    filebase: meson.project_name(),
    name: meson.project_name(),
    version: meson.project_version(),
    description: 'Manage trace event',
    url: 'https://git.kernel.org/pub/scm/libs/libtrace/libtraceevent.git/')

libtraceevent_dep = declare_dependency(
    include_directories: ['.'],
    link_with: libtraceevent)
