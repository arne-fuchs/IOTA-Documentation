# Hornet

## Building inx-dashboard

1. ```bash 
    git clone https://github.com/iotaledger/inx-dashboard.git
   ```
2. ```bash
    cd inx-dashboard
   ```
3. ```bash
   git submodule update --init --recursive
   ```
4. ```bash
   cd node-dashboard
   ```
5. ```bash
   npm install && npm run build
   ```
6. ```bash
   cd ..
   ```
7. ```bash
   ./build.sh && go build
   ```