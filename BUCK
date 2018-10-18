http_archive(
  name = 'pixman-0.34.0', 
  out = 'out', 
  urls = [
    'https://www.cairographics.org/releases/pixman-0.34.0.tar.gz', 
  ], 
  type = 'tar.gz', 
  sha256 = '21b6b249b51c6800dc9553b65106e1e37d0e25df942c90531d4c3997aa20a88e', 
  strip_prefix = 'pixman-0.34.0', 
)

genrule(
  name = 'make', 
  out = 'out', 
  cmd = ' && '.join([
    'mkdir -p $OUT', 
    'cp -r $(location :pixman-0.34.0)/. $TMP', 
    'cd $TMP', 
    './configure --prefix=$OUT', 
    'make', 
    'make install', 
  ]), 
)

genrule(
  name = 'pixman-h', 
  out = 'pixman.h', 
  cmd = 'cp $(location :make)/include/pixman-1/pixman.h $OUT', 
)

genrule(
  name = 'pixman-version-h', 
  out = 'pixman-version.h', 
  cmd = 'cp $(location :make)/include/pixman-1/pixman-version.h $OUT', 
)

genrule(
  name = 'pixman-static-linux', 
  out = 'libpixman-1.a', 
  cmd = 'cp $(location :make)/lib/libpixman-1.a $OUT', 
)

genrule(
  name = 'pixman-shared-linux', 
  out = 'libpixman-1.so', 
  cmd = 'cp $(location :make)/lib/libpixman-1.so $OUT', 
)

prebuilt_cxx_library(
  name = 'pixman', 
  header_namespace = '', 
  exported_headers = {
    'pixman.h': ':pixman-h', 
    'pixman-version.h': ':pixman-version-h', 
  }, 
  platform_static_lib = [
    ('^linux.*', ':pixman-static-linux'), 
  ], 
  platform_shared_lib = [
    ('^linux.*', ':pixman-shared-linux'), 
  ], 
  visibility = [
    'PUBLIC', 
  ], 
)
