# Maintainer: jebez <jeremy.bezairie@gmail.com>
pkgname=cpn-mvn
pkgver=1
pkgrel=1
pkgdesc='Copy or move handling same file name by incremental directory'
arch=('x86_64')
url='https://github.com/jebez1/cpn-mvn'
options=('!debug')

build() {
  cd "$srcdir"

  cat << 'EOF' > cpn.cpp
#include <filesystem>
#include <string>

namespace fs = std::filesystem;

int main(int argc, char* argv[]) {
    if (argc < 3) return 1;

    fs::path dest_dir = argv[argc - 1];
    std::error_code ec;
    
    if (!fs::exists(dest_dir)) fs::create_directories(dest_dir, ec);

    for (int i = 1; i < argc - 1; ++i) {
        std::string src_str = argv[i];
        while (src_str.length() > 1 && src_str.back() == '/') src_str.pop_back();
        
        fs::path src = src_str;
        if (!fs::exists(src)) continue;

        fs::path filename = src.filename();
        fs::path target = dest_dir / filename;

        if (fs::exists(target)) {
            int n = 0;
            while (true) {
                fs::path n_dir = dest_dir / std::to_string(n);
                fs::path n_target = n_dir / filename;
                
                if (!fs::exists(n_target)) {
                    if (!fs::exists(n_dir)) fs::create_directories(n_dir, ec);
                    target = n_target;
                    break;
                }
                n++;
            }
        }
        
        fs::copy(src, target, fs::copy_options::recursive | fs::copy_options::copy_symlinks, ec);
    }
    return 0;
}
EOF

  cat << 'EOF' > mvn.cpp
#include <filesystem>
#include <string>

namespace fs = std::filesystem;

int main(int argc, char* argv[]) {
    if (argc < 3) return 1;

    fs::path dest_dir = argv[argc - 1];
    std::error_code ec;
    
    if (!fs::exists(dest_dir)) fs::create_directories(dest_dir, ec);

    for (int i = 1; i < argc - 1; ++i) {
        std::string src_str = argv[i];
        while (src_str.length() > 1 && src_str.back() == '/') src_str.pop_back();
        
        fs::path src = src_str;
        if (!fs::exists(src)) continue;

        fs::path filename = src.filename();
        fs::path target = dest_dir / filename;

        if (fs::exists(target)) {
            int n = 0;
            while (true) {
                fs::path n_dir = dest_dir / std::to_string(n);
                fs::path n_target = n_dir / filename;
                
                if (!fs::exists(n_target)) {
                    if (!fs::exists(n_dir)) fs::create_directories(n_dir, ec);
                    target = n_target;
                    break;
                }
                n++;
            }
        }
        
        fs::rename(src, target, ec);
        
        // If rename fails (e.g., cross-device link), fallback to copy + delete
        if (ec) {
            ec.clear();
            fs::copy(src, target, fs::copy_options::recursive | fs::copy_options::copy_symlinks, ec);
            if (!ec) fs::remove_all(src, ec);
        }
    }
    return 0;
}
EOF

  g++ -O3 cpn.cpp -o cpn -std=c++17
  g++ -O3 mvn.cpp -o mvn -std=c++17
}

package() {
  cd "$srcdir"
  install -Dm755 cpn "$pkgdir/usr/bin/cpn"
  install -Dm755 mvn "$pkgdir/usr/bin/mvn"
}