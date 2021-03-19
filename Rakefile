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
  puts "--- mruby_compiler ---"
  dir = "#{MRBGEMS_DIR}/mruby-compiler"
  build_dir = "#{BUILD_MRBGEMS_DIR}/mruby-compiler"
  prefix = "gem_compiler"

  cp "#{dir}/core/codegen.c", "#{SRC_DIR}/#{prefix}_codegen.c"
  cp "#{dir}/core/y.tab.c", "#{SRC_DIR}/#{prefix}_y.tab.c"
  sh "patch -p0 --no-backup-if-mismatch < #{prefix}_y.tab.c.patch"
  cp "#{dir}/core/node.h", INCLUDE_DIR
  cp "#{dir}/core/lex.def", INCLUDE_DIR
end

def mrb_copy(name)
  puts "--- mruby-#{name} ---"
  dir = "#{MRBGEMS_DIR}/mruby-#{name}"
  build_dir = "#{BUILD_MRBGEMS_DIR}/mruby-#{name}"

  prefix = "gem_" + name.gsub("-", "_")

  Dir.glob("#{dir}/src/*").each do |filename|
    cp filename, File.join(SRC_DIR, prefix + "_" + File.basename(filename))
  end

  cp "#{build_dir}/gem_init.c", "#{SRC_DIR}/#{prefix}_init.c"
end

def mruby_require
  puts "--- mruby_require ---"
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

  # stdlib.gembox
  mrb_copy("compar-ext")
  mrb_copy("enum-ext")
  mrb_copy("string-ext")
  mrb_copy("numeric-ext")
  mrb_copy("array-ext")
  mrb_copy("hash-ext")
  mrb_copy("range-ext")
  mrb_copy("proc-ext")
  mrb_copy("symbol-ext")
  mrb_copy("object-ext")
  mrb_copy("objectspace")
  mrb_copy("fiber")
  mrb_copy("enumerator")
  mrb_copy("enum-lazy")
  mrb_copy("toplevel-ext")
  mrb_copy("kernel-ext")
  mrb_copy("class-ext")

  # math.gembox
  mrb_copy("math")
  mrb_copy("rational")
  mrb_copy("complex")

  mrb_copy("print")

  # special setting gems
  mruby_compiler
  mruby_require
end

task :clean do
  rm_rf "#{PACK_DIR}"
end
