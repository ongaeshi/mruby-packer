# encoding: utf-8
require 'fileutils'
require 'rake'

MRUBY_DIR = "../mruby"
MRBGEMS_DIR = "#{MRUBY_DIR}/mrbgems"
MRBGEMS_REPOS_DIR = "#{MRUBY_DIR}/build/repos/host"
BUILD_MRBGEMS_DIR = "#{MRUBY_DIR}/build/host/mrbgems"

PACK_DIR = "mruby"
SRC_DIR = "#{PACK_DIR}/src"
INCLUDE_DIR = "#{PACK_DIR}/include"

def mruby_compiler
  dir = "#{MRBGEMS_DIR}/mruby-compiler"
  build_dir = "#{BUILD_MRBGEMS_DIR}/mruby-compiler"
  prefix = "gem_compiler"

  cp "#{dir}/core/codegen.c", "#{SRC_DIR}/#{prefix}_codegen.c"
  cp "#{dir}/core/y.tab.c", "#{SRC_DIR}/#{prefix}_y.tab.c"
  sh "patch -p0 --no-backup-if-mismatch < #{prefix}_y.tab.c.patch"
  cp "#{dir}/core/node.h", INCLUDE_DIR
  cp "#{dir}/core/lex.def", INCLUDE_DIR
end

def mrb_c(name)
  dir = "#{MRBGEMS_DIR}/mruby-#{name}"
  build_dir = "#{BUILD_MRBGEMS_DIR}/mruby-#{name}"
  prefix = "gem_#{name}"

  cp "#{dir}/src/#{name}.c", "#{SRC_DIR}/#{prefix}_#{name}.c"
  cp "#{build_dir}/gem_init.c", "#{SRC_DIR}/#{prefix}_init.c"
end

def mruby_print = mrb_c("print")
def mruby_fiber = mrb_c("fiber")

def mruby_require
  dir = "#{MRBGEMS_REPOS_DIR}/mruby-require"
  build_dir = "#{BUILD_MRBGEMS_DIR}/mruby-require"
  prefix = "gem_require"

  cp "#{dir}/src/mrb_require.c", "#{SRC_DIR}/#{prefix}_require.c"
  cp "#{build_dir}/gem_init.c", "#{SRC_DIR}/#{prefix}_init.c"
end

task :default => :all

task :all do
  mkdir_p PACK_DIR
  mkdir_p SRC_DIR
  mkdir_p INCLUDE_DIR

  # mruby
  cp FileList["#{MRUBY_DIR}/src/*.c"], SRC_DIR
  cp "#{MRUBY_DIR}/build/host/mrblib/mrblib.c", SRC_DIR
  cp_r "#{MRUBY_DIR}/include", "#{INCLUDE_DIR}/.."
  cp FileList["#{MRUBY_DIR}/src/*.h"], INCLUDE_DIR
  cp FileList["#{MRUBY_DIR}/build/host/include/mruby/presym/*.h"], "#{INCLUDE_DIR}/mruby/presym"
  cp "gem_init.c", SRC_DIR
  sh "patch -p0 --no-backup-if-mismatch < fmt_fp.c.patch"

  # gems
  mruby_compiler
  mruby_print
  mruby_fiber
  mruby_require
end

task :clean do
  rm_rf "#{PACK_DIR}"
end
