# encoding: utf-8
require 'fileutils'
require 'rake'

MRUBY_DIR = "../mruby"
PACK_DIR = "mruby"
SRC_DIR = "#{PACK_DIR}/src"
INCLUDE_DIR = "#{PACK_DIR}/include"

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
end

task :clean do
  rm_rf "#{PACK_DIR}"
end
