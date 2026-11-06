# Encore platform

This is an internal system refrain selected by an Encore target kit. User
packages cannot depend on `sys@platform` directly; public platform APIs are
provided by `std`.

The package contains target-specific IO, filesystem, networking, process,
thread, clock, TLS, and runtime implementations.

The Linux runtime now includes the HTTP async ABI used by `std::http` and
requires OpenSSL and c-ares >=1.34 development headers and libraries. Linux
programs link against `ssl`, `crypto`, and `cares`, even when HTTP is unused.
Cross-build sysroots need the same dependencies. The initial async backend is
Linux-only; other targets provide unsupported-operation stubs.

`async_waker_hooks.h` lets native backends unregister requests before an
executor destroys its waker event. Hooks have process lifetime and must finish
detaching every reference before returning.

## SDK development

Encore pins this package as a Git submodule. Clone the complete SDK with
`git clone --recurse-submodules --branch trunk https://github.com/encore-ecosystem/encore.git`
and edit this package under `packages/platform/`. Sibling `path@` dependencies
are resolved from that checkout. Encore CI tests the exact package commits
with the compiler on all supported native targets. Published package archives
rewrite path dependencies to exact index versions.
