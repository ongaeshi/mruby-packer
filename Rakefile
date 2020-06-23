# encoding: utf-8
require 'fileutils'
require 'rake'

MRUBY_DIR = "../mruby"
MRBGEMS_DIR = "#{MRUBY_DIR}/mrbgems"
BUILD_MRBGEMS_DIR = "#{MRUBY_DIR}/build/host/mrbgems"

PACK_DIR = "mruby"
SRC_DIR = "#{PACK_DIR}/src"
INCLUDE_DIR = "#{PACK_DIR}/include"

def mruby_compiler
  dir = "#{MRBGEMS_DIR}/mruby-compiler"
  prefix = "gem_compiler"

  cp "#{dir}/core/codegen.c", "#{SRC_DIR}/#{prefix}_codegen.c"
  cp "#{dir}/core/y.tab.c", "#{SRC_DIR}/#{prefix}_y.tab.c"
  cp "#{dir}/core/node.h", INCLUDE_DIR
  cp "#{dir}/core/lex.def", INCLUDE_DIR
end

def mruby_print
  dir = "#{MRBGEMS_DIR}/mruby-print"
  build_dir = "#{BUILD_MRBGEMS_DIR}/mruby-print"
  prefix = "gem_print"

  cp "#{dir}/src/print.c", "#{SRC_DIR}/#{prefix}_print.c"
  cp "#{build_dir}/gem_init.c", "#{SRC_DIR}/#{prefix}_init.c"
end

task :default => :pack

task :pack do
  mkdir_p PACK_DIR
  mkdir_p SRC_DIR
  mkdir_p INCLUDE_DIR

  # mruby
  cp FileList["#{MRUBY_DIR}/src/*.c"], SRC_DIR
  cp "#{MRUBY_DIR}/build/host/mrblib/mrblib.c", SRC_DIR
  cp_r "#{MRUBY_DIR}/include", "#{INCLUDE_DIR}/.."
  cp FileList["#{MRUBY_DIR}/src/*.h"], INCLUDE_DIR
  cp "gem_init.c", SRC_DIR

  # gems
  mruby_compiler
  mruby_print
end

task :clean do
  rm_rf "#{PACK_DIR}"
end
