VERSION = "1.24.0"

hashes = {
    "darwin_amd64": "10ba63f78cd622601d83f53f4603a4e5d946ac2f331eba3af2a1627dc59ca908",
    "darwin_arm64": "3b886e6b00c4e6cdaa633752439860d771f6cc93720fdaf899ef01d5991e7473",
    "freebsd_amd64": "becb883a13c7f00eca71219a8539c5cc80eead8e0104e07c2d02ec6c6900ec7d",
    "linux_amd64": "92fbc45047eb825c0bded235447b9c455716f270ce174d107798f5770a4201ab",
    "linux_arm64": "bec438aa687021ad97ab1d626b538644b0a8827784e6458d24939e726f3db9c5",
    "windows_amd64": "52cbc448ccfe3796339016db0f0790969fae8d90fe7490c313237301e23e21f5",
}

# Upstream publishes no windows_amd64 please_go, and without one a native Windows plz cannot
# build a Go target at all - the tool it needs has nothing to download. This fork publishes that
# one asset and nothing else, so only that architecture is redirected here. When upstream
# publishes its own, this whole map entry goes back to being unremarkable.
owners = {"windows_amd64": "PeterNeiss"}

# The Windows build carries fixes the other platforms' builds do not need, so it is released under its
# own version rather than replacing an asset earlier commits pin by hash.
versions = {"windows_amd64": "1.24.0-windows.3"}

for a, h in hashes.items():
    native = f"{CONFIG.OS}_{CONFIG.ARCH}" == a
    owner = owners.get(a, "please-build")
    version = versions.get(a, VERSION)
    remote_file(
        name = "please_go" if native else f"please_go_{a}",
        binary = True,
        hashes = [h],
        # The downloaded name carries the version and platform, which on Windows leaves it with
        # no extension in PATHEXT - and Go's exec will not run such a file even given its full
        # path. Every other platform is happy with the name it arrives under.
        out = "please_go.exe" if a.startswith("windows") else None,
        url = f"https://github.com/{owner}/go-rules/releases/download/please-go-v{version}/please_go-{version}-{a}",
        visibility = ["PUBLIC"] if native else None,
    )
