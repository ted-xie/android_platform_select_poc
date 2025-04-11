load("@rules_cc//cc:cc_library.bzl", "cc_library")

platform(
    name = "x86_android",
    constraint_values = [
        "@platforms//cpu:x86_32",
        "@platforms//os:android",
    ],
)

genrule(
  name = "asdf_cc",
  outs = ["asdf.cc"],
  cmd = """cat << EOF > $@
#include <pthread.h>
#include <stdio.h>

void *workload(void *thing) {
  union {
    void* v;
    long l;
  } u;
  u.v = thing;
  u.l = 4321;

  return thing;
}

int asdf() {
  pthread_t tid;
  int x = 1234;
  pthread_create(&tid, NULL, workload, (void*) &x);
  pthread_join(tid, NULL);
  return x;
}
EOF"""
)

cc_library(
  name = "asdf",
  srcs = [":asdf_cc"],
  linkopts = select({
    "@platforms//os:android": [],
    "//conditions:default": ["-lpthread"],
  }),
)
