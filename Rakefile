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

# stdlib.gembox
def mruby_compar_ext = mrb_copy("compar-ext")
def mruby_enum_ext = mrb_copy("enum-ext")
def mruby_string_ext = mrb_copy("string-ext")
def mruby_numeric_ext = mrb_copy("numeric-ext")
def mruby_array_ext = mrb_copy("array-ext")
def mruby_hash_ext = mrb_copy("hash-ext")
def mruby_range_ext = mrb_copy("range-ext")
def mruby_proc_ext = mrb_copy("proc-ext")
def mruby_symbol_ext = mrb_copy("symbol-ext")
def mruby_object_ext = mrb_copy("object-ext")
def mruby_objectspace = mrb_copy("objectspace")
def mruby_fiber = mrb_copy("fiber")
def mruby_enumerator = mrb_copy("enumerator")
def mruby_enum_lazy = mrb_copy("enum-lazy")
def mruby_toplevel_ext = mrb_copy("toplevel-ext")
def mruby_kernel_ext = mrb_copy("kernel-ext")
def mruby_class_ext = mrb_copy("class-ext")

# math.gembox
def mruby_math = mrb_copy("math")
# def mruby_rational
# def mruby_complex

def mruby_print = mrb_copy("print")
    
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

  # special
  mruby_compiler
  mruby_require
end

task :clean do
  rm_rf "#{PACK_DIR}"
end
