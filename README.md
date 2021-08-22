# app-theta
Theta wallet application framework for Nano S and Nano X.
Ledger Blue is not maintained anymore, but the app can still be compiled for this target using the branch `blue-final-release`.

This app follows the specification available in the `doc/` folder.

To compile it and load it on a device, please check out our [developer portal](https://developers.ledger.com/docs/NA/start_here/).

# Testing

Testing is done via the open-source framework [zemu](https://github.com/Zondax/zemu).

## Running tests

First [install yarn](https://classic.yarnpkg.com/en/docs/install/#debian-stable).
Then you can install the project by simply running:
```
make test
```
This will run `make install_tests` and `make run_tests`

To run a specific tests (here the send test):
```
cd tests
sudo jest --runInBand --detectOpenHandles src/send.test.js
```

Make sure you're in the `tests` folder before running `jest` or `yarn test`.


## Adding tests

### Zemu

To add tests, copy one of the already existing test files in `tests/src/`.
You then need to adapt the `buffer` and `tx` variables to adapt to the APDU you wish to send.

- Adapt the expected screen flow. Please create a folder under `tests/snapshots` with the name of the test you're performing.
- Then adapt the `ORIGINAL_SNAPSHOT_PATH_PREFIX` with the name of the folder you just created.
- To create the snapshots, modify the `SNAPSHOT_PATH_PREFIX` and set it to be equal to `ORIGINAL_SNAPSHOT_PATH_PREFIX`.
- Run the tests once, this will create all the snapshots in the folder you created.
- Put back your `SNAPSHOT_PATH_PREFIX` to `snapshots/tmp/`.

Finally make sure you adapt the expected signature!

### Update binaries

Don't forget to update the binaries in the test folder. To do so, compile with those environement variables:
```
make DEBUG=1 ALLOW_DATA=1
```

Then copy the binary to the `tests/elfs` folder (in this case, compiled with SDK for nanoS):
```
cp bin/app.elf tests/elfs/theta_nanos.elf
```

Repeat the operation for a binary compiled with nanoX SDK and change for `theta_nanox.elf`.