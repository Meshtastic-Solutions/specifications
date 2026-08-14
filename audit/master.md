# Device behavior audit — `master` branch

One entry per variant: **171 variants** across 10 platform families, with the effective (post-include-chain) defines, the behavior those defines produce in the consuming code, and the raw defines they were resolved from.

Generated from `meshtastic_device_behavior_audit.xlsx` (sheet “Master”). See [the audit overview](README.md) for method, semantics, and caveats.

🟢 = variant exists only on `master` · 🟡 = defines differ from `develop`

## Platforms

| Platform | Variants |
|---|---:|
| `esp32s3` | 61 |
| `nrf52840` | 51 |
| `esp32` | 31 |
| `rp2040` | 10 |
| `esp32c3` | 6 |
| `stm32` | 5 |
| `esp32c6` | 2 |
| `native` | 2 |
| `rp2350` | 2 |
| `esp32s2` | 1 |

## Overview

A condensed index — pin numbers are the resolved GPIO, blank means the variant does not declare that feature, and `+` means it declares more in that field than fits here. Variant names link to the full entry below, which is authoritative.

| Variant | User button | Alt/back | Cancel | Touch | Status LED | Polarity | Notif LED | Buzzer | Screen |
|---|---|---|---|---|---|---|---|---|---|
| [`esp32/betafpv_2400_tx_micro`](#esp32betafpv_2400_tx_micro) | `25` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/betafpv_900_tx_nano`](#esp32betafpv_900_tx_nano) | `25` |  |  |  | `16` | `1 (default)` |  |  |  |
| [`esp32/chatter2`](#esp32chatter2) |  |  |  |  |  | `1 (default)` |  | `19` |  |
| [`esp32/diy/9m2ibr_aprs_lora_tracker`](#esp32diy9m2ibr_aprs_lora_tracker) | `15` |  |  |  | `13` | `1 (default)` |  | `33` |  |
| [`esp32/diy/dr-dev`](#esp32diydr-dev) | `13` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/diy/hydra`](#esp32diyhydra) | `39` |  |  |  | `2` | `1 (default)` |  |  |  |
| [`esp32/diy/v1`](#esp32diyv1) | `39` |  |  |  | `2` | `1 (default)` |  |  |  |
| [`esp32/diy/v1_1`](#esp32diyv1_1) | `2` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/hackerboxes_esp32_io`](#esp32hackerboxes_esp32_io) | `0` |  |  |  | `2` | `1` |  |  |  |
| [`esp32/heltec_v1`](#esp32heltec_v1) | `0` |  |  |  | `25` | `1 (default)` |  |  |  |
| [`esp32/heltec_v2`](#esp32heltec_v2) | `0` |  |  |  | `25` | `1 (default)` |  |  |  |
| [`esp32/heltec_v2.1`](#esp32heltec_v21) | `0` |  |  |  | `25` | `1 (default)` |  |  |  |
| [`esp32/heltec_wireless_bridge`](#esp32heltec_wireless_bridge) |  |  |  |  | `22` | `1 (default)` |  |  |  |
| [`esp32/heltec_wsl_v2.1`](#esp32heltec_wsl_v21) | `0` |  |  |  | `LED` | `1 (default)` |  |  |  |
| [`esp32/m5stack_core`](#esp32m5stack_core) | `38` |  |  |  |  | `1 (default)` |  | `25` |  |
| [`esp32/m5stack_coreink`](#esp32m5stack_coreink) | `5` |  |  |  | `10` | `1` |  | `2` |  |
| [`esp32/nano-g1`](#esp32nano-g1) | `36` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/nano-g1-explorer`](#esp32nano-g1-explorer) | `36` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/radiomaster_900_bandit`](#esp32radiomaster_900_bandit) | `34` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/radiomaster_900_bandit_nano`](#esp32radiomaster_900_bandit_nano) |  |  |  |  | `15` | `1 (default)` |  |  |  |
| [`esp32/rak11200`](#esp32rak11200) |  |  |  |  | `2` | `1 (default)` |  |  |  |
| [`esp32/station-g1`](#esp32station-g1) | `36` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/tbeam`](#esp32tbeam) 🟡 | `38` |  |  |  | `4 +` | `0` |  |  | `1` |
| [`esp32/tbeam_v07`](#esp32tbeam_v07) | `39` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32/tlora_v1`](#esp32tlora_v1) | `0` |  |  |  | `2` | `1 (default)` |  |  |  |
| [`esp32/tlora_v1_3`](#esp32tlora_v1_3) | `36` |  |  |  | `25` | `1 (default)` |  |  |  |
| [`esp32/tlora_v2`](#esp32tlora_v2) | `\` |  |  |  | `25` | `1 (default)` |  |  |  |
| [`esp32/tlora_v2_1_16`](#esp32tlora_v2_1_16) | `0` |  |  |  | `25` | `1 (default)` |  | `25` |  |
| [`esp32/tlora_v2_1_18`](#esp32tlora_v2_1_18) | `12` |  |  |  | `25` | `1 (default)` |  |  |  |
| [`esp32/trackerd`](#esp32trackerd) | `0` |  |  |  | `13` | `1 (default)` |  |  |  |
| [`esp32/wiphone`](#esp32wiphone) |  |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32c3/ai-c3`](#esp32c3ai-c3) | `9` |  |  |  | `30` | `1 (default)` |  |  |  |
| [`esp32c3/diy/esp32c3_super_mini`](#esp32c3diyesp32c3_super_mini) | `9` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32c3/hackerboxes_esp32c3_oled`](#esp32c3hackerboxes_esp32c3_oled) | `9` |  |  |  | `8` | `1` |  |  |  |
| [`esp32c3/heltec_esp32c3`](#esp32c3heltec_esp32c3) | `9` |  |  |  | `2` | `1` |  |  |  |
| [`esp32c3/heltec_hru_3601`](#esp32c3heltec_hru_3601) | `9` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32c3/m5stack-stamp-c3`](#esp32c3m5stack-stamp-c3) | `3` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32c6/m5stack_unitc6l`](#esp32c6m5stack_unitc6l) |  |  |  |  |  | `1 (default)` |  | `11` |  |
| [`esp32c6/tlora_c6`](#esp32c6tlora_c6) |  |  |  |  | `7` | `0` |  |  |  |
| [`esp32s2/nugget_s2_lora`](#esp32s2nugget_s2_lora) | `0` |  |  |  | `15` | `1 (default)` |  |  |  |
| [`esp32s3/CDEBYTE_EoRa-Hub`](#esp32s3cdebyte_eora-hub) | `0` |  |  |  | `35` | `1 (default)` |  |  |  |
| [`esp32s3/CDEBYTE_EoRa-S3`](#esp32s3cdebyte_eora-s3) | `0` |  |  |  | `37` | `1 (default)` |  |  |  |
| [`esp32s3/EBYTE_ESP32-S3`](#esp32s3ebyte_esp32-s3) | `0` |  |  |  | `1` | `1` |  | `11` |  |
| [`esp32s3/ELECROW-ThinkNode-G3`](#esp32s3elecrow-thinknode-g3) | `4` |  |  |  |  | `0` |  |  |  |
| [`esp32s3/ELECROW-ThinkNode-M2`](#esp32s3elecrow-thinknode-m2) | `47` | `4` |  |  | `6` | `1 (default)` |  | `5` |  |
| [`esp32s3/ELECROW-ThinkNode-M5`](#esp32s3elecrow-thinknode-m5) | `21` | `14` |  |  |  | `1 (default)` |  | `9` |  |
| [`esp32s3/ELECROW-ThinkNode-M7`](#esp32s3elecrow-thinknode-m7) | `4` |  |  |  |  | `0` |  |  |  |
| [`esp32s3/bpi_picow_esp32_s3`](#esp32s3bpi_picow_esp32_s3) | `40` |  |  |  | `46` | `0` |  |  |  |
| [`esp32s3/crowpanel-esp32s3-5-epaper`](#esp32s3crowpanel-esp32s3-5-epaper) | `2` |  |  |  | `41` | `1 (default)` |  | `18` |  |
| [`esp32s3/diy/my_esp32s3_diy_eink`](#esp32s3diymy_esp32s3_diy_eink) | `0` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/diy/my_esp32s3_diy_oled`](#esp32s3diymy_esp32s3_diy_oled) | `0` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/diy/t-energy-s3_e22`](#esp32s3diyt-energy-s3_e22) | `39` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/dreamcatcher`](#esp32s3dreamcatcher) | `0` |  |  |  | `6` | `1` |  | `48` |  |
| [`esp32s3/elecrow_panel`](#esp32s3elecrow_panel) |  |  |  |  |  | `1 (default)` |  | `8` |  |
| [`esp32s3/esp32-s3-pico`](#esp32s3esp32-s3-pico) | `0` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/hackaday-communicator`](#esp32s3hackaday-communicator) |  |  |  |  |  | `0` | `1` |  |  |
| [`esp32s3/heltec_capsule_sensor_v3`](#esp32s3heltec_capsule_sensor_v3) | `18` |  |  |  | `33` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_sensor_hub`](#esp32s3heltec_sensor_hub) | `17` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/heltec_v3`](#esp32s3heltec_v3) | `0` |  |  |  | `LED` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_v4`](#esp32s3heltec_v4) | `0` | `35` |  |  | `35` | `1 (default)` |  | `6` | `1` |
| [`esp32s3/heltec_v4_r8`](#esp32s3heltec_v4_r8) | `0` | `PIN_BUTTON2` |  |  | `46` | `1 (default)` |  | `4` | `1` |
| [`esp32s3/heltec_vision_master_e213`](#esp32s3heltec_vision_master_e213) | `0` | `21` |  |  | `45` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_vision_master_e290`](#esp32s3heltec_vision_master_e290) | `0` | `21` |  |  | `45` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_vision_master_t190`](#esp32s3heltec_vision_master_t190) | `0` | `21` |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/heltec_wireless_paper`](#esp32s3heltec_wireless_paper) | `0` |  |  |  | `18` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_wireless_paper_v1`](#esp32s3heltec_wireless_paper_v1) | `0` |  |  |  | `18` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_wireless_tracker`](#esp32s3heltec_wireless_tracker) | `0` |  |  |  | `18` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_wireless_tracker_V1_0`](#esp32s3heltec_wireless_tracker_v1_0) | `0` |  |  |  | `18` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_wireless_tracker_v2`](#esp32s3heltec_wireless_tracker_v2) | `0` |  |  |  | `18` | `1 (default)` |  |  |  |
| [`esp32s3/heltec_wsl_v3`](#esp32s3heltec_wsl_v3) | `0` |  |  |  | `LED` | `1 (default)` |  |  |  |
| [`esp32s3/icarus`](#esp32s3icarus) | `7` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/link32_s3_v1`](#esp32s3link32_s3_v1) | `0` |  | `36` |  |  | `1 (default)` |  |  |  |
| [`esp32s3/m5stack_cardputer_adv`](#esp32s3m5stack_cardputer_adv) | `0` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/m5stack_cores3`](#esp32s3m5stack_cores3) |  |  |  |  | `derived (AXP2101)` | `1 (default)` |  |  |  |
| [`esp32s3/mesh-tab`](#esp32s3mesh-tab) | `0` |  |  |  | `21` | `1 (default)` |  | `5` | `1` |
| [`esp32s3/meshnology-w10`](#esp32s3meshnology-w10) | `0` |  |  |  | `derived (AXP2101)` | `1 (default)` |  |  |  |
| [`esp32s3/mini-epaper-s3`](#esp32s3mini-epaper-s3) |  |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/nibble_esp32`](#esp32s3nibble_esp32) | `0` |  |  |  | `1` | `1 (default)` |  |  |  |
| [`esp32s3/nugget_s3_lora`](#esp32s3nugget_s3_lora) | `43` |  |  |  | `15` | `1 (default)` |  |  |  |
| [`esp32s3/picomputer-s3`](#esp32s3picomputer-s3) | `0` |  |  |  |  | `1 (default)` |  | `43` |  |
| [`esp32s3/rak3312`](#esp32s3rak3312) |  |  |  |  | `46` | `1` | `45` |  |  |
| [`esp32s3/rak_wismesh_tap_v2`](#esp32s3rak_wismesh_tap_v2) | `0` |  |  |  | `46` | `1` | `45` | `38` |  |
| [`esp32s3/seeed-sensecap-indicator`](#esp32s3seeed-sensecap-indicator) | `38` |  |  |  |  | `1 (default)` |  |  | `1` |
| [`esp32s3/seeed_xiao_s3`](#esp32s3seeed_xiao_s3) | `21` |  |  |  | `48` | `1` |  |  |  |
| [`esp32s3/station-g2`](#esp32s3station-g2) | `38` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/station-g3`](#esp32s3station-g3) | `38` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/t-beam-1w`](#esp32s3t-beam-1w) | `0` | `17` |  |  | `18` | `1` |  |  |  |
| [`esp32s3/t-deck`](#esp32s3t-deck) |  |  |  |  |  | `1 (default)` |  |  | `1` |
| [`esp32s3/t-deck-pro`](#esp32s3t-deck-pro) | `0` |  |  |  |  | `1 (default)` |  |  | `1` |
| [`esp32s3/t-deck-pro-v1_1`](#esp32s3t-deck-pro-v1_1) | `0` |  |  |  |  | `1 (default)` |  |  | `1` |
| [`esp32s3/t-eth-elite`](#esp32s3t-eth-elite) | `0` |  |  |  | `38` | `1 (default)` |  |  |  |
| [`esp32s3/t-watch-s3`](#esp32s3t-watch-s3) 🟡 | `0` |  |  |  | `derived (AXP2101)` | `1 (default)` |  |  | `1` |
| [`esp32s3/t5s3_epaper`](#esp32s3t5s3_epaper) 🟡 | `48` | `0` |  |  |  | `1 (default)` |  |  | `1` |
| [`esp32s3/tbeam-s3-core`](#esp32s3tbeam-s3-core) 🟡 | `0` |  |  |  | `derived (AXP2101)` | `1 (default)` |  |  |  |
| [`esp32s3/tlora-pager`](#esp32s3tlora-pager) | `0` |  |  |  |  | `1 (default)` |  |  |  |
| [`esp32s3/tlora_t3s3_epaper`](#esp32s3tlora_t3s3_epaper) | `0` |  |  |  | `37` | `1 (default)` |  |  |  |
| [`esp32s3/tlora_t3s3_v1`](#esp32s3tlora_t3s3_v1) | `0` |  |  |  | `37` | `1 (default)` |  |  |  |
| [`esp32s3/tracksenger/internal`](#esp32s3tracksengerinternal) | `0` |  |  |  | `18` | `1 (default)` |  | `43` |  |
| [`esp32s3/tracksenger/lcd`](#esp32s3tracksengerlcd) | `0` |  |  |  | `18` | `1 (default)` |  | `43` |  |
| [`esp32s3/tracksenger/oled`](#esp32s3tracksengeroled) | `0` |  |  |  | `18` | `1 (default)` |  | `43` |  |
| [`esp32s3/unphone`](#esp32s3unphone) | `0` | `21` | `45` |  | `13` | `0` |  |  | `1` |
| [`native/portduino`](#nativeportduino) |  |  |  |  |  | `1 (default)` |  |  |  |
| [`native/portduino-buildroot`](#nativeportduino-buildroot) |  |  |  |  |  | `1 (default)` |  |  |  |
| [`nrf52840/Dongle_nRF52840-pca10059-v1`](#nrf52840dongle_nrf52840-pca10059-v1) | `38` |  |  |  | `6` | `0` |  |  |  |
| [`nrf52840/ELECROW-ThinkNode-M1`](#nrf52840elecrow-thinknode-m1) | `42` | `39` |  |  | `36` | `1` | `13` | `6` |  |
| [`nrf52840/ELECROW-ThinkNode-M3`](#nrf52840elecrow-thinknode-m3) | `12` |  |  |  | `33` | `LOW` | `35` | `23` |  |
| [`nrf52840/ELECROW-ThinkNode-M4`](#nrf52840elecrow-thinknode-m4) | `4` |  |  |  |  | `1` |  |  |  |
| [`nrf52840/ELECROW-ThinkNode-M6`](#nrf52840elecrow-thinknode-m6) | `17` |  |  |  | `12` | `HIGH` | `7` |  |  |
| [`nrf52840/ME25LS01-4Y10TD`](#nrf52840me25ls01-4y10td) | `27` |  |  |  | `39` | `1` |  | `25` |  |
| [`nrf52840/ME25LS01-4Y10TD_e-ink`](#nrf52840me25ls01-4y10td_e-ink) | `27` |  |  |  | `39` | `1` |  | `25` |  |
| [`nrf52840/MS24SF1`](#nrf52840ms24sf1) | `-1` |  |  |  | `-1` | `1` |  |  |  |
| [`nrf52840/MakePython_nRF52840_eink`](#nrf52840makepython_nrf52840_eink) | `47` |  |  |  | `42` | `0` |  |  |  |
| [`nrf52840/MakePython_nRF52840_oled`](#nrf52840makepython_nrf52840_oled) | `47` |  |  |  | `42` | `0` |  |  |  |
| [`nrf52840/TWC_mesh_v4`](#nrf52840twc_mesh_v4) | `34` |  |  |  | `42` | `0` |  |  |  |
| [`nrf52840/canaryone`](#nrf52840canaryone) | `PIN_BUTTON1((GPIO_PORT0 + 15))` | `PIN_BUTTON2((GPIO_PORT0 + 16))` |  |  | `PIN_LED1((GPIO_PORT1 + 1))` | `0` |  |  |  |
| [`nrf52840/diy/nrf52_promicro_diy_tcxo`](#nrf52840diynrf52_promicro_diy_tcxo) | `32` |  |  |  | `15` | `1` |  |  |  |
| [`nrf52840/dls_Minimesh_Lite`](#nrf52840dls_minimesh_lite) | `32` |  |  |  | `15` | `1` |  |  |  |
| [`nrf52840/feather_diy`](#nrf52840feather_diy) | `34` |  |  |  | `47` | `1` |  |  |  |
| [`nrf52840/gat562_mesh_trial_tracker`](#nrf52840gat562_mesh_trial_tracker) | `9` | `12` |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/heltec_mesh_node_t096`](#nrf52840heltec_mesh_node_t096) | `42` |  |  |  | `28` | `1` |  |  |  |
| [`nrf52840/heltec_mesh_node_t1`](#nrf52840heltec_mesh_node_t1) | `42` | `14` |  |  | `16` | `0` |  | `9` |  |
| [`nrf52840/heltec_mesh_node_t114`](#nrf52840heltec_mesh_node_t114) | `42` |  |  |  | `35` | `0` |  |  |  |
| [`nrf52840/heltec_mesh_node_t114-inkhud`](#nrf52840heltec_mesh_node_t114-inkhud) | `42` |  |  |  | `35` | `0` |  |  |  |
| [`nrf52840/heltec_mesh_pocket`](#nrf52840heltec_mesh_pocket) | `42` |  |  |  | `13` | `0` |  |  |  |
| [`nrf52840/heltec_mesh_solar`](#nrf52840heltec_mesh_solar) | `42` |  |  |  | `47` | `0` |  |  |  |
| [`nrf52840/meshlink`](#nrf52840meshlink) | `-1` |  |  |  | `24` | `0` |  | `31` |  |
| [`nrf52840/meshtiny`](#nrf52840meshtiny) |  |  | `9` |  | `35` | `1` |  | `30` |  |
| [`nrf52840/monteops_hw1`](#nrf52840monteops_hw1) |  | `12` |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/muzi_base`](#nrf52840muzi_base) |  |  | `15` |  | `35` | `0` | `36` | `22` |  |
| [`nrf52840/nano-g2-ultra`](#nrf52840nano-g2-ultra) | `38` |  |  |  |  | `0` |  |  |  |
| [`nrf52840/r1-neo`](#nrf52840r1-neo) | `26` |  |  |  | `36` | `1` | `28` | `3` |  |
| [`nrf52840/rak2560`](#nrf52840rak2560) | `9` | `12` |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/rak3401_1watt`](#nrf52840rak3401_1watt) |  |  |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/rak4631`](#nrf52840rak4631) | `9` | `12` |  |  | `35` | `1` | `36` | `21` |  |
| [`nrf52840/rak4631_epaper`](#nrf52840rak4631_epaper) | `9` | `12` |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/rak4631_epaper_onrxtx`](#nrf52840rak4631_epaper_onrxtx) | `9` |  |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/rak4631_eth_gw`](#nrf52840rak4631_eth_gw) | `9` | `12` |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/rak4631_nomadstar_meteor_pro`](#nrf52840rak4631_nomadstar_meteor_pro) | `9` | `12` |  |  | `35` | `1` | `36` |  |  |
| [`nrf52840/rak_wismeshtag`](#nrf52840rak_wismeshtag) | `9` | `12` |  |  | `35` | `1` | `36` | `21` |  |
| [`nrf52840/rak_wismeshtap`](#nrf52840rak_wismeshtap) | `9` | `12` |  |  | `35` | `1` | `36` |  | `1` |
| [`nrf52840/seeed_mesh_tracker_X1`](#nrf52840seeed_mesh_tracker_x1) | `6` |  |  |  | `24` | `1` |  | `25` |  |
| [`nrf52840/seeed_solar_node`](#nrf52840seeed_solar_node) | `13` |  |  | `20` | `12` | `1` |  |  |  |
| [`nrf52840/seeed_wio_tracker_L1`](#nrf52840seeed_wio_tracker_l1) 🟡 |  |  | `13` |  | `11` | `1` |  | `12` |  |
| [`nrf52840/seeed_wio_tracker_L1_eink`](#nrf52840seeed_wio_tracker_l1_eink) 🟡 | `13` |  | `13` |  | `11` | `1` |  | `12` |  |
| [`nrf52840/seeed_xiao_nrf52840_kit`](#nrf52840seeed_xiao_nrf52840_kit) | `D0` |  |  |  | `13` | `0` |  |  |  |
| [`nrf52840/t-echo`](#nrf52840t-echo) | `42` | `18` |  | `11` | `14` | `0` |  |  |  |
| [`nrf52840/t-echo-card`](#nrf52840t-echo-card) | `42` |  |  |  |  | `1` |  | `38` |  |
| [`nrf52840/t-echo-lite`](#nrf52840t-echo-lite) | `24` | `18` |  |  | `39` | `0` |  |  |  |
| [`nrf52840/t-echo-plus`](#nrf52840t-echo-plus) | `42` | `18` |  | `11` | `14` | `0` |  | `6` |  |
| [`nrf52840/t-impulse-plus`](#nrf52840t-impulse-plus) |  |  |  | `18` |  | `1` | `19` |  |  |
| [`nrf52840/tracker-t1000-e`](#nrf52840tracker-t1000-e) | `6` |  |  |  | `24` | `1` |  | `25 +` |  |
| [`nrf52840/wio-sdk-wm1110`](#nrf52840wio-sdk-wm1110) | `23` |  |  |  | `13` | `1` |  |  |  |
| [`nrf52840/wio-t1000-s`](#nrf52840wio-t1000-s) | `6` |  |  |  | `24` | `1` |  | `25 +` |  |
| [`nrf52840/wio-tracker-wm1110`](#nrf52840wio-tracker-wm1110) | `34` |  |  |  | `6` | `0` |  |  |  |
| [`rp2040/challenger_2040_lora`](#rp2040challenger_2040_lora) | `0xFFFFFFFF` |  |  |  |  | `1 (default)` |  |  |  |
| [`rp2040/ec_catsniffer`](#rp2040ec_catsniffer) |  |  |  |  | `27` | `1 (default)` |  |  |  |
| [`rp2040/feather_rp2040_rfm95`](#rp2040feather_rp2040_rfm95) | `7` |  |  |  | `PIN_LED` | `1 (default)` |  |  |  |
| [`rp2040/nibble_rp2040`](#rp2040nibble_rp2040) | `-1` |  |  |  | `1` | `1 (default)` |  |  |  |
| [`rp2040/rak11310`](#rp2040rak11310) | `9` |  |  |  | `PIN_LED1` | `1 (default)` |  |  |  |
| [`rp2040/rp2040-lora`](#rp2040rp2040-lora) | `-1` |  |  |  | `PIN_LED` | `1 (default)` |  |  |  |
| [`rp2040/rpipico`](#rp2040rpipico) | `17` |  |  |  | `PIN_LED` | `1 (default)` |  |  |  |
| [`rp2040/rpipico-slowclock`](#rp2040rpipico-slowclock) | `18` |  |  |  | `PIN_LED` | `1 (default)` |  |  |  |
| [`rp2040/rpipicow`](#rp2040rpipicow) | `17` |  |  |  | `PIN_LED` | `1 (default)` |  |  |  |
| [`rp2040/senselora_rp2040`](#rp2040senselora_rp2040) | `2` |  |  |  | `PIN_LED1` | `1 (default)` |  |  |  |
| [`rp2350/rpipico2`](#rp2350rpipico2) | `17` |  |  |  | `PIN_LED` | `1 (default)` |  |  |  |
| [`rp2350/rpipico2w`](#rp2350rpipico2w) | `17` |  |  |  |  | `1 (default)` |  |  |  |
| [`stm32/CDEBYTE_E77-MBL`](#stm32cdebyte_e77-mbl) |  |  |  |  | `PB4` | `1` |  |  |  |
| [`stm32/milesight_gs301`](#stm32milesight_gs301) | `PC13` |  |  |  | `PIN_LED1` | `1` |  | `PA6` |  |
| [`stm32/rak3172`](#stm32rak3172) |  |  |  |  | `PA0` | `1` |  |  |  |
| [`stm32/russell`](#stm32russell) | `PH3` |  |  |  | `PA0` | `1` |  |  |  |
| [`stm32/wio-e5`](#stm32wio-e5) |  |  |  |  | `PB5` | `0` |  |  |  |

## Variants

### esp32/betafpv_2400_tx_micro

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=25` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=16`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 25; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Ambient NeoPixel strip: 1 px on 16 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 25
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 16
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32/betafpv_900_tx_nano

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=25` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=16` |

**Behavior**

- User button on 25; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 16 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 25
LED_POWER = 16
```

</details>

### esp32/chatter2

Platform `esp32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Buzzer | `PIN_BUZZER=19` |

**Behavior**

- Buzzer on 19: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
PIN_BUZZER = 19
```

</details>

### esp32/diy/9m2ibr_aprs_lora_tracker

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=15` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=13` |
| Buzzer | `PIN_BUZZER=33` |

**Behavior**

- User button on 15; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 13 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): USER_LED
- Buzzer on 33: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 15
LED_POWER = 13
PIN_BUZZER = 33
USER_LED = 2
```

</details>

### esp32/diy/dr-dev

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=13` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=12` |

**Behavior**

- User button on 13; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 13
EXT_NOTIFY_OUT = 12
```

</details>

### esp32/diy/hydra

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=39` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=2` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=12` |

**Behavior**

- User button on 39; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 2 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 39
EXT_NOTIFY_OUT = 12
LED_POWER = 2
```

</details>

### esp32/diy/v1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=39` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=2` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=12` |

**Behavior**

- User button on 39; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 2 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 39
EXT_NOTIFY_OUT = 12
LED_POWER = 2
```

</details>

### esp32/diy/v1_1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=2` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=12` |

**Behavior**

- User button on 2; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 2
EXT_NOTIFY_OUT = 12
```

</details>

### esp32/hackerboxes_esp32_io

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=2` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 2 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 2
LED_STATE_ON = 1
```

</details>

### esp32/heltec_v1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 25
RESET_OLED = 16
```

</details>

### esp32/heltec_v2

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 25
RESET_OLED = 16
```

</details>

### esp32/heltec_v2.1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
EXT_NOTIFY_OUT = 13
LED_POWER = 25
RESET_OLED = 16
```

</details>

### esp32/heltec_wireless_bridge

Platform `esp32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=22` |
| WiFi LED | `WIFI_LED=23` |
| BLE LED | `BLE_LED=25` |

**Behavior**

- Power/status LED on 22 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- WiFi LED on 23 (on while connected)
- BLE LED on 25 (on while BLE connected)

<details><summary>All matched defines (raw)</summary>

```c
BLE_LED = 25
LED_POWER = 22
WIFI_LED = 23
```

</details>

### esp32/heltec_wsl_v2.1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=LED` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = 0
LED_POWER = LED
```

</details>

### esp32/m5stack_core

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=38` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Buzzer | `PIN_BUZZER=25` |

**Behavior**

- User button on 38; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Buzzer on 25: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 38
PIN_BUZZER = 25
```

</details>

### esp32/m5stack_coreink

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=5` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=10` |
| Buzzer | `PIN_BUZZER=2` |

**Behavior**

- User button on 5; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 10 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 2: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 5
LED_POWER = 10
LED_STATE_ON = 1
PIN_BUZZER = 2
PIN_EINK_EN = -1
```

</details>

### esp32/nano-g1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=36` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 36; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 36
EXT_NOTIFY_OUT = 13
```

</details>

### esp32/nano-g1-explorer

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=36` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 36; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 36
EXT_NOTIFY_OUT = 13
```

</details>

### esp32/radiomaster_900_bandit

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=34` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=15`<br>`NEOPIXEL_COUNT=6`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 34; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Ambient NeoPixel strip: 6 px on 15 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 34
ENABLE_AMBIENTLIGHTING
HAS_NEOPIXEL
NEOPIXEL_COUNT = 6
NEOPIXEL_DATA = 15
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32/radiomaster_900_bandit_nano

Platform `esp32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=15` |

**Behavior**

- Power/status LED on 15 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_POWER = 15
```

</details>

### esp32/rak11200

Platform `esp32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=LED_BLUE (= 2)` |

**Behavior**

- Power/status LED on LED_BLUE (= 2) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_GREEN

<details><summary>All matched defines (raw)</summary>

```c
LED_BLUE = 2
LED_GREEN = 12
LED_POWER = LED_BLUE (= 2)
WB_LED1 = 12
WB_LED2 = 2
```

</details>

### esp32/station-g1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=36` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 36; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 36
EXT_NOTIFY_OUT = 13
```

</details>

### esp32/tbeam

Platform `esp32` · 🟡 **DIFFERS: PMU_POWER_BUTTON_IS_CANCEL, VARIANT_TOUCHSCREEN**

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=38` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=true` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=4`<br>`HAS_PMU=derived (AXP192)` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 38; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=true
- Power/status LED on 4 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- PMU (AXP) charge LED driven by StatusLEDModule
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = true
BUTTON_PIN = 38
EXT_NOTIFY_OUT = 13
HAS_AXP192
HAS_TOUCHSCREEN = 1
LED_POWER = 4
LED_STATE_ON = 0
TOUCH_IRQ = -1
VTFT_LEDA = 4
```

</details>

### esp32/tbeam_v07

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=39` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 39; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 39
EXT_NOTIFY_OUT = 13
```

</details>

### esp32/tlora_v1

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=2` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=13` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 2 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
EXT_NOTIFY_OUT = 13
LED_POWER = 2
RESET_OLED = 16
```

</details>

### esp32/tlora_v1_3

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=36` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |

**Behavior**

- User button on 36; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 36
LED_POWER = 25
RESET_OLED = 16
```

</details>

### esp32/tlora_v2

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=\` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |

**Behavior**

- User button on \; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = \
LED_POWER = 25
```

</details>

### esp32/tlora_v2_1_16

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |
| Buzzer | `PIN_BUZZER=25` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 25: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 25
PIN_BUZZER = 25
```

</details>

### esp32/tlora_v2_1_18

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=12` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=25` |

**Behavior**

- User button on 12; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 25 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 12
LED_POWER = 25
```

</details>

### esp32/trackerd

Platform `esp32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=13` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 13 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
LED_POWER = 13
PIN_LED1 = 13
PIN_LED2 = 15
PIN_LED3 = 2
ledOff = (pin) pinMode(pin, INPUT)
```

</details>

### esp32/wiphone

Platform `esp32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

### esp32c3/ai-c3

Platform `esp32c3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=9` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=30` |

**Behavior**

- User button on 9; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 30 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 9
LED_POWER = 30
```

</details>

### esp32c3/diy/esp32c3_super_mini

Platform `esp32c3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=9` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 9; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 9
```

</details>

### esp32c3/hackerboxes_esp32c3_oled

Platform `esp32c3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=9` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=8` |

**Behavior**

- User button on 9; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 8 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 9
LED_POWER = 8
LED_STATE_ON = 1
```

</details>

### esp32c3/heltec_esp32c3

Platform `esp32c3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=9` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=2` |

**Behavior**

- User button on 9; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 2 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 9
LED_POWER = 2
LED_STATE_ON = 1
```

</details>

### esp32c3/heltec_hru_3601

Platform `esp32c3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=9` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=2`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 9; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Ambient NeoPixel strip: 1 px on 2 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 9
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 2
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32c3/m5stack-stamp-c3

Platform `esp32c3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=3` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 3; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 3
```

</details>

### esp32c6/m5stack_unitc6l

Platform `esp32c6`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=2`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |
| Buzzer | `PIN_BUZZER=11` |

**Behavior**

- Ambient NeoPixel strip: 1 px on 2 (AmbientLighting module)
- Buzzer on 11: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_EXTENDER
ENABLE_AMBIENTLIGHTING
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 2
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
OLED_TINY
PIN_BUZZER = 11
```

</details>

### esp32c6/tlora_c6

Platform `esp32c6`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=7` |

**Behavior**

- Power/status LED on 7 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_POWER = 7
LED_STATE_ON = 0
```

</details>

### esp32s2/nugget_s2_lora

Platform `esp32s2`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=15` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=12`<br>`NEOPIXEL_COUNT=3`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 15 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Ambient NeoPixel strip: 3 px on 12 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
HAS_NEOPIXEL
LED_POWER = 15
NEOPIXEL_COUNT = 3
NEOPIXEL_DATA = 12
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/CDEBYTE_EoRa-Hub

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=35` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 35 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = 0
LED_POWER = 35
```

</details>

### esp32s3/CDEBYTE_EoRa-S3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=37` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 37 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 37
```

</details>

### esp32s3/EBYTE_ESP32-S3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=1` |
| Buzzer | `PIN_BUZZER=11` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=2` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 1 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 11: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
EXT_NOTIFY_OUT = 2
LED_POWER = 1
LED_STATE_ON = 1
PIN_BUZZER = 11
```

</details>

### esp32s3/ELECROW-ThinkNode-G3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=4` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| WiFi LED | `WIFI_LED=5` |

**Behavior**

- User button on 4; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- WiFi LED on 5 (on while connected)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 4
LED_PIN = 6
LED_STATE_ON = 0
WIFI_LED = 5
```

</details>

### esp32s3/ELECROW-ThinkNode-M2

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1 (= 47)` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2 (= 4)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=6` |
| Buzzer | `PIN_BUZZER=5` |

**Behavior**

- User button on PIN_BUTTON1 (= 47); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2 (= 4): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on 6 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 5: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_ACTIVE_LOW = false
ALT_BUTTON_ACTIVE_PULLUP = false
ALT_BUTTON_PIN = PIN_BUTTON2 (= 4)
BUTTON_PIN = PIN_BUTTON1 (= 47)
LED_POWER = 6
PIN_BUTTON1 = 47
PIN_BUTTON2 = 4
PIN_BUZZER = 5
```

</details>

### esp32s3/ELECROW-ThinkNode-M5

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1 (= 21)` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(14)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Buzzer | `PIN_BUZZER=9` |

**Behavior**

- User button on PIN_BUTTON1 (= 21); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2(14): click=ALT_PRESS, 500ms hold=ALT_LONG
- PCA_LED_POWER=3 on PCA I/O expander
- PCA_LED_ENABLE=2 on PCA I/O expander
- PCA_LED_NOTIFICATION=1 on PCA I/O expander
- Buzzer on 9: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_WAKE
BUTTON_PIN = PIN_BUTTON1 (= 21)
BUTTON_PIN_ALT = PIN_BUTTON2 (= 14)
PCA_LED_ENABLE = 2
PCA_LED_NOTIFICATION = 1
PCA_LED_POWER = 3
PCA_PIN_EINK_EN = 5
PIN_BUTTON1 = 21
PIN_BUTTON2 = 14
PIN_BUZZER = 9
POWER_LED_HARDWARE_BLINKS_WHILE_CHARGING
```

</details>

### esp32s3/ELECROW-ThinkNode-M7

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=4` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=true` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Pairing LED | `LED_PAIRING=46` |
| LoRa RX LED | `LED_LORA=46` |

**Behavior**

- User button on 4; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s=SHUTDOWN, 15s=FACTORY_RESET, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=true
- BLE pairing LED on 46: unpaired=2s blink, pairing=1s blink, connected=solid; auto-off after 30s
- LoRa RX LED on 46: 100ms flash per received packet

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = true
BUTTON_PIN = 4
LED_LORA = 46
LED_PAIRING = 46
LED_PIN = 3
LED_STATE_OFF = 1
LED_STATE_ON = 0
```

</details>

### esp32s3/bpi_picow_esp32_s3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=40` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=46` |

**Behavior**

- User button on 40; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 46 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 40
LED_POWER = 46
LED_STATE_ON = 0
```

</details>

### esp32s3/crowpanel-esp32s3-5-epaper

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=2` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=41` |
| Buzzer | `PIN_BUZZER=(0 + 18) = 18` |

**Behavior**

- User button on 2; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 41 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on (0 + 18) = 18: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 2
LED_POWER = 41
PIN_BUZZER = (0 + 18) = 18
```

</details>

### esp32s3/diy/my_esp32s3_diy_eink

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=38`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Ambient NeoPixel strip: 1 px on 38 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 38
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/diy/my_esp32s3_diy_oled

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=38`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Ambient NeoPixel strip: 1 px on 38 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 38
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/diy/t-energy-s3_e22

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=39` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 39; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 39
```

</details>

### esp32s3/dreamcatcher

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=6` |
| Buzzer | `PIN_BUZZER=48` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 6 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 48: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 6
LED_STATE_ON = 1
PIN_BUZZER = 48
RESET_OLED = 8
```

</details>

### esp32s3/elecrow_panel

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Buzzer | `PIN_BUZZER=8` |

**Behavior**

- Buzzer on 8: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
LGFX_TOUCH = FT5x06
LGFX_TOUCH_I2C_ADDR = 0x38 = 56
LGFX_TOUCH_I2C_SCL = 16
LGFX_TOUCH_I2C_SDA = 15
LGFX_TOUCH_INT = 47
LGFX_TOUCH_ROTATION = 0
LGFX_TOUCH_RST = 48
PIN_BUZZER = 8
SCREEN_TOUCH_INT = 47
USE_PIN_BUZZER
WAKE_ON_TOUCH
```

</details>

### esp32s3/esp32-s3-pico

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=21`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Ambient NeoPixel strip: 1 px on 21 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
EXT_NOTIFY_OUT = 22
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 21
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/hackaday-communicator

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Notification LED | `LED_NOTIFICATION=1` |

**Behavior**

- Notification LED on 1 (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
LED_NOTIFICATION = 1
LED_STATE_ON = 0
```

</details>

### esp32s3/heltec_capsule_sensor_v3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=18` |
| Btn active-low | `BUTTON_ACTIVE_LOW=false` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=33` |

**Behavior**

- User button on 18; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=false, activePullup=false
- Power/status LED on 33 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_ACTIVE_LOW = false
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = 18
DEBUG_DISABLED
LED_POWER = 33
LED_POWER2 = 34
```

</details>

### esp32s3/heltec_sensor_hub

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=17` |
| Btn active-low | `BUTTON_ACTIVE_LOW=false` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=18`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 17; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=false, activePullup=false
- Ambient NeoPixel strip: 1 px on 18 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_ACTIVE_LOW = false
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = 17
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 18
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/heltec_v3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=LED` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = 0
LED_POWER = LED
RESET_OLED = RST_OLED
```

</details>

### esp32s3/heltec_v4

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(35)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=35` |
| Buzzer | `PIN_BUZZER=6` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2(35): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on 35 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 6: ExternalNotification default (PWM, alert on message) + system beeps
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_PIN = 0
CUSTOM_TOUCH_DRIVER
HAS_TOUCHSCREEN = 1
HELTEC_V4_OLED
INPUTDRIVER_BUTTON_TYPE = 0
LED_POWER = 35
PIN_BUTTON2 = 35
PIN_BUZZER = 6
RESET_OLED = 21
SCREEN_TOUCH_INT = TOUCH_INT_PIN
SCREEN_TOUCH_RST = TOUCH_RST_PIN
TOUCH_I2C_PORT = 1
TOUCH_INT_PIN = -1
TOUCH_RST_PIN = 44
TOUCH_SCL_PIN = I2C_SCL1
TOUCH_SDA_PIN = I2C_SDA1
TOUCH_SLAVE_ADDRESS = 0x2E
USE_PIN_BUZZER = PIN_BUZZER
```

</details>

### esp32s3/heltec_v4_r8

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=46` |
| Buzzer | `PIN_BUZZER=4` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2: click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on 46 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 4: ExternalNotification default (PWM, alert on message) + system beeps
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_ACTIVE_LOW = false
ALT_BUTTON_PIN = PIN_BUTTON2
BUTTON_PIN = 0
CUSTOM_TOUCH_DRIVER
HAS_TOUCHSCREEN = 1
HELTEC_V4_R8_OLED
INPUTDRIVER_BUTTON_TYPE = 0
LED_POWER = 46
PIN_BUTTON2 = 46
PIN_BUZZER = 4
RESET_OLED = 21
SCREEN_TOUCH_INT = TOUCH_INT_PIN
SCREEN_TOUCH_RST = TOUCH_RST_PIN
TOUCH_I2C_PORT = 0
TOUCH_INT_PIN = -1
TOUCH_RST_PIN = -1
TOUCH_SCL_PIN = I2C_SCL
TOUCH_SDA_PIN = I2C_SDA
TOUCH_SLAVE_ADDRESS = 0x2E
USE_PIN_BUZZER = PIN_BUZZER
```

</details>

### esp32s3/heltec_vision_master_e213

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2 (= 21)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=45` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2 (= 21): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on 45 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu) | button1 on PIN_BUTTON2 (= 21) -> ShortPress: next tile + chirp

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
ALT_BUTTON_PIN = PIN_BUTTON2 (= 21)
BUTTON_PIN = 0
LED_POWER = 45
PIN_BUTTON2 = 21
```

</details>

### esp32s3/heltec_vision_master_e290

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2 (= 21)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=45` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2 (= 21): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on 45 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu) | button1 on PIN_BUTTON2 (= 21) -> ShortPress: next tile + chirp

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
ALT_BUTTON_PIN = PIN_BUTTON2 (= 21)
BUTTON_CLICK_MS = 200
BUTTON_PIN = 0
LED_POWER = 45
PIN_BUTTON2 = 21
```

</details>

### esp32s3/heltec_vision_master_t190

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2 (= 21)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2 (= 21): click=ALT_PRESS, 500ms hold=ALT_LONG

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
ALT_BUTTON_PIN = PIN_BUTTON2 (= 21)
BUTTON_PIN = 0
PIN_BUTTON2 = 21
VTFT_LEDA = 17
```

</details>

### esp32s3/heltec_wireless_paper

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = 0
LED_POWER = 18
```

</details>

### esp32s3/heltec_wireless_paper_v1

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = 0
LED_POWER = 18
```

</details>

### esp32s3/heltec_wireless_tracker

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
DEBUG_DISABLED
LED_POWER = 18
```

</details>

### esp32s3/heltec_wireless_tracker_V1_0

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
DEBUG_DISABLED
LED_POWER = 18
```

</details>

### esp32s3/heltec_wireless_tracker_v2

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
LED_POWER = 18
```

</details>

### esp32s3/heltec_wsl_v3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=LED` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = 0
LED_POWER = LED
```

</details>

### esp32s3/icarus

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=7` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 7; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 7
```

</details>

### esp32s3/link32_s3_v1

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Cancel button | `CANCEL_BUTTON_PIN=36` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=35`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Cancel button on 36: click=CANCEL, 4s hold=SHUTDOWN
- Ambient NeoPixel strip: 1 px on 35 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
CANCEL_BUTTON_ACTIVE_LOW = true
CANCEL_BUTTON_ACTIVE_PULLUP = true
CANCEL_BUTTON_PIN = 36
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 35
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/m5stack_cardputer_adv

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=21`<br>`NEOPIXEL_COUNT=1`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Ambient NeoPixel strip: 1 px on 21 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
HAS_NEOPIXEL
NEOPIXEL_COUNT = 1
NEOPIXEL_DATA = 21
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
VTFT_LEDA = 38
```

</details>

### esp32s3/m5stack_cores3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `HAS_PMU=derived (AXP2101)` |

**Behavior**

- PMU (AXP) charge LED driven by StatusLEDModule

<details><summary>All matched defines (raw)</summary>

```c
HAS_AXP2101
```

</details>

### esp32s3/mesh-tab

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=21` |
| Buzzer | `PIN_BUZZER=5` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 21 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 5: ExternalNotification default (PWM, alert on message) + system beeps
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
HAS_TOUCHSCREEN = 1
LED_POWER = 21
LGFX_TOUCH = XPT2046
LGFX_TOUCH_CLK = 12
LGFX_TOUCH_CS = 7
LGFX_TOUCH_DIN = 13
LGFX_TOUCH_DO = 11
LGFX_TOUCH_I2C_ADDR = 0x38 = 56
LGFX_TOUCH_I2C_FREQ = 400000
LGFX_TOUCH_I2C_PORT = 0
LGFX_TOUCH_I2C_SCL = 9
LGFX_TOUCH_I2C_SDA = 8
LGFX_TOUCH_INT = 41
LGFX_TOUCH_ROTATION = 4
LGFX_TOUCH_RST = 7
LGFX_TOUCH_SPI_FREQ = 2500000
LGFX_TOUCH_SPI_HOST = 2
LGFX_TOUCH_X_MAX = 3900
LGFX_TOUCH_X_MIN = 300
LGFX_TOUCH_Y_MAX = 3900
LGFX_TOUCH_Y_MIN = 400
PIN_BUZZER = 5
USE_PIN_BUZZER
```

</details>

### esp32s3/meshnology-w10

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `HAS_PMU=derived (AXP2101)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- PMU (AXP) charge LED driven by StatusLEDModule

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
HAS_AXP2101
```

</details>

### esp32s3/mini-epaper-s3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on INPUTDRIVER_TWO_WAY_ROCKER_BTN (= 0) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
PIN_EINK_EN = 42
```

</details>

### esp32s3/nibble_esp32

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 1 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
LED_POWER = 1
```

</details>

### esp32s3/nugget_s3_lora

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=43` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=15` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=10`<br>`NEOPIXEL_COUNT=3`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on 43; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 15 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Ambient NeoPixel strip: 3 px on 10 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 43
ENABLE_AMBIENTLIGHTING
HAS_NEOPIXEL
LED_POWER = 15
NEOPIXEL_COUNT = 3
NEOPIXEL_DATA = 10
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
```

</details>

### esp32s3/picomputer-s3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Buzzer | `PIN_BUZZER=43` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Buzzer on 43: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
PIN_BUZZER = 43
USE_PIN_BUZZER = PIN_BUZZER (= 43)
```

</details>

### esp32s3/rak3312

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=LED_GREEN (= 46)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 45)` |

**Behavior**

- Power/status LED on LED_GREEN (= 46) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 45) (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
LED_BLUE = 45
LED_GREEN = 46
LED_NOTIFICATION = LED_BLUE (= 45)
LED_POWER = LED_GREEN (= 46)
LED_STATE_ON = 1
PIN_LED1 = LED_GREEN (= 46)
ledOff = (pin) pinMode(pin, INPUT)
```

</details>

### esp32s3/rak_wismesh_tap_v2

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=LED_GREEN (= 46)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 45)` |
| Buzzer | `PIN_BUZZER=38` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on LED_GREEN (= 46) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 45) (ExternalNotification default output, 1000ms on message)
- Buzzer on 38: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
INPUTDRIVER_BUTTON_TYPE = 0
LED_BLUE = 45
LED_GREEN = 46
LED_NOTIFICATION = LED_BLUE (= 45)
LED_POWER = LED_GREEN (= 46)
LED_STATE_ON = 1
LGFX_TOUCH = FT5x06
LGFX_TOUCH_I2C_ADDR = 0x38 = 56
LGFX_TOUCH_I2C_FREQ = 100000
LGFX_TOUCH_I2C_PORT = 0
LGFX_TOUCH_I2C_SCL = 40
LGFX_TOUCH_I2C_SDA = 9
LGFX_TOUCH_INT = 39
LGFX_TOUCH_ROTATION = 2
LGFX_TOUCH_RST = -1
LGFX_TOUCH_X_MAX = 239
LGFX_TOUCH_X_MIN = 0
LGFX_TOUCH_Y_MAX = 319
LGFX_TOUCH_Y_MIN = 0
PIN_BUZZER = 38
PIN_LED1 = LED_GREEN (= 46)
USE_PIN_BUZZER = PIN_BUZZER (= 38)
ledOff = (pin) pinMode(pin, INPUT)
```

</details>

### esp32s3/seeed-sensecap-indicator

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=38` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=true` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 38; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=true
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = true
BUTTON_PIN = 38
CUSTOM_TOUCH_DRIVER
HAS_TOUCHSCREEN = 1
INPUTDRIVER_BUTTON_TYPE = 38
SCREEN_TOUCH_INT = (6 | IO_EXPANDER)
SCREEN_TOUCH_RST = (7 | IO_EXPANDER)
TOUCH_I2C_PORT = 0
TOUCH_SLAVE_ADDRESS = 0x48 = 72
```

</details>

### esp32s3/seeed_xiao_s3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=21` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=48` |

**Behavior**

- User button on 21; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 48 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 21
LED_POWER = 48
LED_STATE_ON = 1
```

</details>

### esp32s3/station-g2

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=38` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 38; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 38
```

</details>

### esp32s3/station-g3

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=38` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 38; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 38
```

</details>

### esp32s3/t-beam-1w

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Alt/Back button | `ALT_BUTTON_PIN=17` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=18` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on 17: click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_PIN = 17
BUTTON_PIN = 0
LED_POWER = 18
LED_STATE_ON = 1
OLED_HEIGHT = 64
OLED_WIDTH = 128
```

</details>

### esp32s3/t-deck

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=true` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = true
CALIBRATE_TOUCH = 0
CUSTOM_TOUCH_DRIVER
HAS_TOUCHSCREEN = 1
INPUTDRIVER_BUTTON_TYPE = 0
SCREEN_TOUCH_INT = 16
TOUCH_I2C_PORT = 0
TOUCH_SLAVE_ADDRESS = 0x5D
USE_I2S_BUZZER
```

</details>

### esp32s3/t-deck-pro

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Vibration | `PIN_VIBRATION=2` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Vibration motor on 2: ExternalNotification vibra 500ms, nag 2s
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
HAS_TOUCHSCREEN = 1
PIN_VIBRATION = 2
```

</details>

### esp32s3/t-deck-pro-v1_1

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
HAS_TOUCHSCREEN = 1
```

</details>

### esp32s3/t-eth-elite

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=38` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 38 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
LED_POWER = 38
```

</details>

### esp32s3/t-watch-s3

Platform `esp32s3` · 🟡 **DIFFERS: PMU_POWER_BUTTON_IS_CANCEL**

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `HAS_PMU=derived (AXP2101)` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- PMU (AXP) charge LED driven by StatusLEDModule
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
HAS_AXP2101
HAS_TOUCHSCREEN = 1
SCREEN_TOUCH_INT = 16
SCREEN_TOUCH_USE_I2C1
TOUCH_I2C_PORT = 1
TOUCH_SLAVE_ADDRESS = 0x38 = 56
WAKE_ON_TOUCH
```

</details>

### esp32s3/t5s3_epaper

Platform `esp32s3` · 🟡 **DIFFERS: BOARD_PCA9535_BUTTON_MASK, SCREEN_TOUCH_INT, TOUCH_POLL_INTERVAL_ACTIVE, TOUCH_POLL_INTERVAL_ACTIVE_FAST, TOUCH_POLL_INTERVAL_IDLE, TOUCH_POLL_INTERVAL_RELEASE, TOUCH_POLL_INTERVAL_RELEASE_FAST, TOUCH_THRESHOLD_X**

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=48` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2 (= 0)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 48; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2 (= 0): click=ALT_PRESS, 500ms hold=ALT_LONG
- Touchscreen input
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on BUTTON_PIN (= 48) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_PIN = PIN_BUTTON2 (= 0)
BUTTON_PIN = 48
HAS_TOUCHSCREEN = 1
PIN_BUTTON2 = 0
TOUCH_THRESHOLD_X = 60
TOUCH_THRESHOLD_Y = 40
```

</details>

### esp32s3/tbeam-s3-core

Platform `esp32s3` · 🟡 **DIFFERS: PMU_POWER_BUTTON_IS_CANCEL**

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `HAS_PMU=derived (AXP2101)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- PMU (AXP) charge LED driven by StatusLEDModule

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
HAS_AXP2101
```

</details>

### esp32s3/tlora-pager

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0
ENABLE_BUTTON_PULLUP
INPUTDRIVER_BUTTON_TYPE = 0
USE_I2S_BUZZER
```

</details>

### esp32s3/tlora_t3s3_epaper

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=37` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 37 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
LED_POWER = 37
```

</details>

### esp32s3/tlora_t3s3_v1

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=37` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on 37 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
LED_POWER = 37
```

</details>

### esp32s3/tracksenger/internal

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |
| Buzzer | `PIN_BUZZER=43` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 43: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_PIN = 0
LED_POWER = 18
PIN_BUZZER = 43
```

</details>

### esp32s3/tracksenger/lcd

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |
| Buzzer | `PIN_BUZZER=43` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 43: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_PIN = 0
LED_POWER = 18
PIN_BUZZER = 43
```

</details>

### esp32s3/tracksenger/oled

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=18` |
| Buzzer | `PIN_BUZZER=43` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 18 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 43: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_PIN = 0
LED_POWER = 18
PIN_BUZZER = 43
```

</details>

### esp32s3/unphone

Platform `esp32s3`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=21` |
| Cancel button | `CANCEL_BUTTON_PIN=45` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=13` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on 0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on 21: click=ALT_PRESS, 500ms hold=ALT_LONG
- Cancel button on 45: click=CANCEL, 4s hold=SHUTDOWN
- Power/status LED on 13 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_PIN = 21
BUTTON_NEED_PULLUP
BUTTON_PIN = 0
CANCEL_BUTTON_ACTIVE_LOW = true
CANCEL_BUTTON_ACTIVE_PULLUP = true
CANCEL_BUTTON_PIN = 45
HAS_TOUCHSCREEN = 1
INPUTDRIVER_BUTTON_TYPE = 21
LED_POWER = 13
LED_STATE_ON = 0
TOUCH_CS = 38
UNPHONE_TOUCHS = 0
```

</details>

### native/portduino

Platform `native`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

### native/portduino-buildroot

Platform `native`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |

### nrf52840/Dongle_nRF52840-pca10059-v1

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 6) = 38)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 6) = 6)` |
| Discrete RGB (R/G/B) | `RGBLED_RED=(0 + 8) = 8`<br>`RGBLED_GREEN=(32 + 9) = 41`<br>`RGBLED_BLUE=(0 + 12) = 12`<br>`RGBLED_CA` |

**Behavior**

- User button on PIN_BUTTON1((32 + 6) = 38); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((0 + 6) = 6) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Discrete RGB LED R=(0 + 8) = 8,G=(32 + 9) = 41,B=(0 + 12) = 12 (common-anode, inverted PWM)

<details><summary>All matched defines (raw)</summary>

```c
LED_BLUE = PIN_LED2 (= 6)
LED_GREEN = PIN_LED1 (= 6)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 6) = 38
PIN_EINK_EN = (0 + 6) = 6
PIN_LED1 = (0 + 6) = 6
PIN_LED2 = (0 + 6) = 6
RGBLED_BLUE = (0 + 12) = 12
RGBLED_CA
RGBLED_GREEN = (32 + 9) = 41
RGBLED_RED = (0 + 8) = 8
```

</details>

### nrf52840/ELECROW-ThinkNode-M1

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2 (= 39)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=(32 + 4) = 36` |
| Notification LED | `LED_NOTIFICATION=(0 + 13) = 13` |
| Buzzer | `PIN_BUZZER=(0 + 6) = 6` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2 (= 39): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on (32 + 4) = 36 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s; hardware blinks during charge (firmware doesn't toggle)
- Notification LED on (0 + 13) = 13 (ExternalNotification default output, 1000ms on message)
- Buzzer on (0 + 6) = 6: ExternalNotification default (PWM, alert on message) + system beeps
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on PIN_BUTTON2 (= 39) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu) | button1 on PIN_BUTTON1 (= 42) -> Down: backlight peek; LongPress: backlight latch on; ShortPress: backlight off + chirp

<details><summary>All matched defines (raw)</summary>

```c
ALT_BUTTON_ACTIVE_LOW = true
ALT_BUTTON_ACTIVE_PULLUP = true
ALT_BUTTON_PIN = PIN_BUTTON2 (= 39)
LED_NOTIFICATION = (0 + 13) = 13
LED_POWER = (32 + 4) = 36
LED_STATE_ON = 1
PIN_BUTTON1 = (32 + 10) = 42
PIN_BUTTON2 = (32 + 7) = 39
PIN_BUZZER = (0 + 6) = 6
PIN_EINK_EN = (32 + 11) = 43
POWER_LED_HARDWARE_BLINKS_WHILE_CHARGING
```

</details>

### nrf52840/ELECROW-ThinkNode-M3

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=12` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=LOW` |
| Power/status LED | `LED_POWER=LED_RED (= 33)` |
| Pairing LED | `LED_PAIRING=LED_BLUE (= 37)` |
| Notification LED | `LED_NOTIFICATION=LED_GREEN (= 35)` |
| Buzzer | `PIN_BUZZER=23` |

**Behavior**

- User button on 12; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on LED_RED (= 33) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- BLE pairing LED on LED_BLUE (= 37): unpaired=2s blink, pairing=1s blink, connected=solid; auto-off after 30s
- Notification LED on LED_GREEN (= 35) (ExternalNotification default output, 1000ms on message)
- Buzzer on 23: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 12
BUTTON_PIN_ALT = (0 + 12) = 12
LED_BLUE = 37
LED_GREEN = 35
LED_NOTIFICATION = LED_GREEN (= 35)
LED_PAIRING = LED_BLUE (= 37)
LED_POWER = LED_RED (= 33)
LED_RED = 33
LED_STATE_OFF = HIGH
LED_STATE_ON = LOW
PIN_BUZZER = 23
```

</details>

### nrf52840/ELECROW-ThinkNode-M4

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(4)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Heartbeat LED | `LED_HEARTBEAT=(32 + 9) = 41` |
| Pairing LED | `LED_PAIRING=13` |
| Battery gauge LEDs | `Battery_LED_1=15`<br>`Battery_LED_2=17`<br>`Battery_LED_3=(32 + 2) = 34`<br>`Battery_LED_4=(32 + 4) = 36` |

**Behavior**

- User button on PIN_BUTTON1(4); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Dedicated heartbeat LED on (32 + 9) = 41 (1ms blip/s when discharging); charge LED forced off while discharging
- BLE pairing LED on 13: unpaired=2s blink, pairing=1s blink, connected=solid; auto-off after 30s
- 4-LED battery gauge (25/50/75% thresholds), shown 10s after button press or while charging
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE

<details><summary>All matched defines (raw)</summary>

```c
Battery_LED_1 = 15
Battery_LED_2 = 17
Battery_LED_3 = (32 + 2) = 34
Battery_LED_4 = (32 + 4) = 36
LED_BLUE = -1
LED_HEARTBEAT = (32 + 9) = 41
LED_PAIRING = 13
LED_STATE_ON = 1
PIN_BUTTON1 = 4
```

</details>

### nrf52840/ELECROW-ThinkNode-M6

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(17)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=HIGH` |
| Power/status LED | `LED_POWER=12` |
| Pairing LED | `LED_PAIRING=7` |
| Notification LED | `LED_NOTIFICATION=LED_PAIRING (= 7)` |

**Behavior**

- User button on PIN_BUTTON1(17); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 12 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- BLE pairing LED on 7: unpaired=2s blink, pairing=1s blink, connected=solid; auto-off after 30s
- Notification LED on LED_PAIRING (= 7) (ExternalNotification default output, 1000ms on message)
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = 1
LED_BLUE = -1
LED_NOTIFICATION = LED_PAIRING (= 7)
LED_PAIRING = 7
LED_POWER = 12
LED_STATE_OFF = LOW
LED_STATE_ON = HIGH
PIN_BUTTON1 = 17
```

</details>

### nrf52840/ME25LS01-4Y10TD

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(0 + 27) = 27` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1 (= 39)` |
| Buzzer | `PIN_BUZZER=(0 + 25) = 25` |

**Behavior**

- User button on (0 + 27) = 27; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1 (= 39) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on (0 + 25) = 25: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = (0 + 27) = 27
LED_BLUE = -1
LED_POWER = PIN_LED1 (= 39)
LED_STATE_ON = 1
PIN_BUZZER = (0 + 25) = 25
PIN_LED1 = (32 + 7) = 39
```

</details>

### nrf52840/ME25LS01-4Y10TD_e-ink

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(0 + 27) = 27` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1 (= 39)` |
| Buzzer | `PIN_BUZZER=(0 + 25) = 25` |

**Behavior**

- User button on (0 + 27) = 27; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1 (= 39) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on (0 + 25) = 25: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = (0 + 27) = 27
LED_BLUE = -1
LED_POWER = PIN_LED1 (= 39)
LED_STATE_ON = 1
PIN_BUZZER = (0 + 25) = 25
PIN_LED1 = (32 + 7) = 39
```

</details>

### nrf52840/MS24SF1

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=-1` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1 (= -1)` |

**Behavior**

- User button on -1; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1 (= -1) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = -1
LED_BLUE = -1
LED_POWER = PIN_LED1 (= -1)
LED_STATE_ON = 1
PIN_LED1 = -1
```

</details>

### nrf52840/MakePython_nRF52840_eink

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 15) = 47)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 10) = 42)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 15) = 47); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 10) = 42) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_GREEN = PIN_LED1 (= 42)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 15) = 47
PIN_EINK_EN
PIN_LED1 = (32 + 10) = 42
```

</details>

### nrf52840/MakePython_nRF52840_oled

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 15) = 47)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 10) = 42)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 15) = 47); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 10) = 42) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_GREEN = PIN_LED1 (= 42)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 15) = 47
PIN_LED1 = (32 + 10) = 42
```

</details>

### nrf52840/TWC_mesh_v4

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 2) = 34)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 10) = 42)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 2) = 34); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 10) = 42) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE

<details><summary>All matched defines (raw)</summary>

```c
LED_BLUE = PIN_LED2 (= 47)
LED_GREEN = PIN_LED1 (= 42)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 2) = 34
PIN_LED1 = (32 + 10) = 42
PIN_LED2 = (32 + 15) = 47
```

</details>

### nrf52840/canaryone

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((GPIO_PORT0 + 15))` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2((GPIO_PORT0 + 16))` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((GPIO_PORT1 + 1))` |

**Behavior**

- User button on PIN_BUTTON1((GPIO_PORT0 + 15)); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2((GPIO_PORT0 + 16)): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1((GPIO_PORT1 + 1)) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE

<details><summary>All matched defines (raw)</summary>

```c
LED_BLUE = PIN_LED1
LED_STATE_ON = 0
PIN_BUTTON1 = (GPIO_PORT0 + 15)
PIN_BUTTON2 = (GPIO_PORT0 + 16)
PIN_LED1 = (GPIO_PORT1 + 1)
PIN_LED2 = (GPIO_PORT0 + 14)
PIN_LED3 = (GPIO_PORT1 + 3)
```

</details>

### nrf52840/diy/nrf52_promicro_diy_tcxo

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(32 + 0) = 32` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 15) = 15)` |

**Behavior**

- User button on (32 + 0) = 32; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((0 + 15) = 15) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = (32 + 0) = 32
LED_BLUE = PIN_LED1 (= 15)
LED_STATE_ON = 1
PIN_LED1 = (0 + 15) = 15
VBAT_RAW_TO_SCALED = (x) (REAL_VBAT_MV_PER_LSB * x)
```

</details>

### nrf52840/dls_Minimesh_Lite

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(32 + 0) = 32` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 15) = 15)` |

**Behavior**

- User button on (32 + 0) = 32; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((0 + 15) = 15) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = (32 + 0) = 32
LED_BLUE = PIN_LED1 (= 15)
LED_STATE_ON = 1
PIN_LED1 = (0 + 15) = 15
VBAT_RAW_TO_SCALED = (x) (REAL_VBAT_MV_PER_LSB * x)
```

</details>

### nrf52840/feather_diy

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(32 + 2) = 34` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 15) = 47)` |

**Behavior**

- User button on (32 + 2) = 34; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 15) = 47) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_GREEN

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = (32 + 2) = 34
LED_BLUE = PIN_LED1 (= 47)
LED_GREEN = PIN_LED2 (= 42)
LED_STATE_ON = 1
PIN_LED1 = (32 + 15) = 47
PIN_LED2 = (32 + 10) = 42
```

</details>

### nrf52840/gat562_mesh_trial_tracker

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_LED1 = 35
```

</details>

### nrf52840/heltec_mesh_node_t096

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 28) = 28)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((0 + 28) = 28) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
LED_BLUE = PIN_LED1 (= 28)
LED_GREEN = PIN_LED1 (= 28)
LED_STATE_ON = 1
PIN_BUTTON1 = (32 + 10) = 42
PIN_LED1 = (0 + 28) = 28
```

</details>

### nrf52840/heltec_mesh_node_t1

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2((0 + 14) = 14)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 16) = 16)` |
| Buzzer | `PIN_BUZZER=(0 + 9) = 9` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2((0 + 14) = 14): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1((0 + 16) = 16) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on (0 + 9) = 9: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
LED_BLUE = PIN_LED1 (= 16)
LED_GREEN = PIN_LED1 (= 16)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 10) = 42
PIN_BUTTON2 = (0 + 14) = 14
PIN_BUZZER = (0 + 9) = 9
PIN_BUZZER_VOLTAGE_MULTIPLIER_1 = (32 + 2) = 34
PIN_BUZZER_VOLTAGE_MULTIPLIER_2 = (32 + 5) = 37
PIN_LED1 = (0 + 16) = 16
```

</details>

### nrf52840/heltec_mesh_node_t114

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 3) = 35)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=14`<br>`NEOPIXEL_COUNT=2`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 3) = 35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Ambient NeoPixel strip: 2 px on 14 (AmbientLighting module)

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
HAS_NEOPIXEL
LED_BLUE = PIN_LED1 (= 35)
LED_GREEN = PIN_LED1 (= 35)
LED_STATE_ON = 0
NEOPIXEL_COUNT = 2
NEOPIXEL_DATA = 14
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
PIN_BUTTON1 = (32 + 10) = 42
PIN_LED1 = (32 + 3) = 35
VTFT_LEDA = 15
```

</details>

### nrf52840/heltec_mesh_node_t114-inkhud

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 3) = 35)` |
| Ambient NeoPixel (data/count/type) | `NEOPIXEL_DATA=14`<br>`NEOPIXEL_COUNT=2`<br>`NEOPIXEL_TYPE=(NEO_GRB + NEO_KHZ800)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 3) = 35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Ambient NeoPixel strip: 2 px on 14 (AmbientLighting module)
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
HAS_NEOPIXEL
LED_BLUE = PIN_LED1 (= 35)
LED_GREEN = PIN_LED1 (= 35)
LED_STATE_ON = 0
NEOPIXEL_COUNT = 2
NEOPIXEL_DATA = 14
NEOPIXEL_TYPE = (NEO_GRB + NEO_KHZ800)
PIN_BUTTON1 = (32 + 10) = 42
PIN_LED1 = (32 + 3) = 35
```

</details>

### nrf52840/heltec_mesh_pocket

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1(13)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1(13) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
LED_BLUE = PIN_LED1 (= 13)
LED_GREEN = PIN_LED1 (= 13)
LED_RED = PIN_LED1 (= 13)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 10) = 42
PIN_LED1 = 13
```

</details>

### nrf52840/heltec_mesh_solar

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 15) = 47)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((32 + 15) = 47) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
HELTEC_MESH_SOLAR_OLED
LED_BLUE = PIN_LED1 (= 47)
LED_GREEN = PIN_LED1 (= 47)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 10) = 42
PIN_LED1 = (32 + 15) = 47
RESET_OLED = 40
VTFT_LEDA = 11
```

</details>

### nrf52840/meshlink

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=-1` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1(24)` |
| Buzzer | `PIN_BUZZER=31` |

**Behavior**

- User button on -1; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1(24) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Buzzer on 31: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = -1
LED_BLUE = PIN_LED1 (= 24)
LED_STATE_ON = 0
PIN_BUZZER = 31
PIN_LED1 = 24
```

</details>

### nrf52840/meshtiny

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Cancel button | `CANCEL_BUTTON_PIN=9` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Buzzer | `PIN_BUZZER=30` |

**Behavior**

- Cancel button on 9: click=CANCEL, 4s hold=SHUTDOWN
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on 30: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
CANCEL_BUTTON_ACTIVE_LOW = true
CANCEL_BUTTON_ACTIVE_PULLUP = false
CANCEL_BUTTON_PIN = 9
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_STATE_ON = 1
PIN_BUZZER = 30
PIN_LED1 = 35
USE_PIN_BUZZER
```

</details>

### nrf52840/monteops_hw1

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |

**Behavior**

- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON2 = 12
PIN_LED1 = 35
```

</details>

### nrf52840/muzi_base

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| Cancel button | `CANCEL_BUTTON_PIN=(0 + 15) = 15` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 3) = 35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| Buzzer | `PIN_BUZZER=(0 + 22) = 22` |

**Behavior**

- Cancel button on (0 + 15) = 15: click=CANCEL, 4s hold=SHUTDOWN
- Power/status LED on PIN_LED1((32 + 3) = 35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- Buzzer on (0 + 22) = 22: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
CANCEL_BUTTON_ACTIVE_LOW = true
CANCEL_BUTTON_ACTIVE_PULLUP = false
CANCEL_BUTTON_PIN = (0 + 15) = 15
LED_BLUE = (32 + 4) = 36
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 0
PIN_BUZZER = (0 + 22) = 22
PIN_LED1 = (32 + 3) = 35
```

</details>

### nrf52840/nano-g2-ultra

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 6) = 38)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=(0 + 4) = 4` |

**Behavior**

- User button on PIN_BUTTON1((32 + 6) = 38); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
EXT_NOTIFY_OUT = (0 + 4) = 4
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 6) = 38
```

</details>

### nrf52840/r1-neo

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(26)` |
| Btn active-low | `BUTTON_ACTIVE_LOW=0` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 4) = 36)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 28)` |
| I2C RGB driver | `HAS_NCP5623` |
| Buzzer | `PIN_BUZZER=(0 + 3) = 3` |

**Behavior**

- User button on PIN_BUTTON1(26); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=0, activePullup=0
- Power/status LED on PIN_LED1((32 + 4) = 36) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 28) (ExternalNotification default output, 1000ms on message)
- NCP5623 I2C RGB LED (AmbientLighting)
- Buzzer on (0 + 3) = 3: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = 0
BUTTON_ACTIVE_PULLUP = 0
BUTTON_SENSE_TYPE = INPUT_SENSE_HIGH
HAS_NCP5623
LED_BLUE = 28
LED_GREEN = PIN_LED1 (= 36)
LED_NOTIFICATION = LED_BLUE (= 28)
LED_STATE_ON = 1
PIN_BUTTON1 = 26
PIN_BUZZER = (0 + 3) = 3
PIN_LED1 = (32 + 4) = 36
```

</details>

### nrf52840/rak2560

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_LED1 = 35
```

</details>

### nrf52840/rak3401_1watt

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |

**Behavior**

- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_LED1 = 35
```

</details>

### nrf52840/rak4631

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| I2C RGB driver | `HAS_NCP5623` |
| Buzzer | `PIN_BUZZER=21` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- NCP5623 I2C RGB LED (AmbientLighting)
- Buzzer on 21: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
HAS_NCP5623
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_BUZZER = 21
PIN_LED1 = 35
```

</details>

### nrf52840/rak4631_epaper

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| I2C RGB driver | `HAS_NCP5623` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- NCP5623 I2C RGB LED (AmbientLighting)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
HAS_NCP5623
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_LED1 = 35
```

</details>

### nrf52840/rak4631_epaper_onrxtx

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| I2C RGB driver | `HAS_NCP5623` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- NCP5623 I2C RGB LED (AmbientLighting)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
HAS_NCP5623
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_EINK_EN = 34
PIN_LED1 = 35
```

</details>

### nrf52840/rak4631_eth_gw

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| I2C RGB driver | `HAS_NCP5623` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- NCP5623 I2C RGB LED (AmbientLighting)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
HAS_NCP5623
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_LED1 = 35
```

</details>

### nrf52840/rak4631_nomadstar_meteor_pro

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| I2C RGB driver | `HAS_LP5562` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- LP5562 I2C RGBW LED (AmbientLighting)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
ENABLE_AMBIENTLIGHTING
HAS_LP5562
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_LED1 = 35
```

</details>

### nrf52840/rak_wismeshtag

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| Buzzer | `PIN_BUZZER=21` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- Buzzer on 21: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_BUZZER = 21
PIN_LED1 = 35
```

</details>

### nrf52840/rak_wismeshtap

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1(9)` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2(12)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(35)` |
| Notification LED | `LED_NOTIFICATION=LED_BLUE (= 36)` |
| I2C RGB driver | `HAS_NCP5623` |
| Touchscreen | `HAS_TOUCHSCREEN=1` |

**Behavior**

- User button on PIN_BUTTON1(9); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Alt/back button on PIN_BUTTON2(12): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1(35) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Notification LED on LED_BLUE (= 36) (ExternalNotification default output, 1000ms on message)
- NCP5623 I2C RGB LED (AmbientLighting)
- Touchscreen input

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
HAS_NCP5623
HAS_TOUCHSCREEN = 1
LED_BLUE = 36
LED_GREEN = PIN_LED1 (= 35)
LED_NOTIFICATION = LED_BLUE (= 36)
LED_STATE_ON = 1
PIN_BUTTON1 = 9
PIN_BUTTON2 = 12
PIN_LED1 = 35
SCREEN_TOUCH_INT = WB_IO6 (= 10)
```

</details>

### nrf52840/seeed_mesh_tracker_X1

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(0 + 6) = 6` |
| Btn active-low | `BUTTON_ACTIVE_LOW=false` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED2 (= 24)` |
| Critical-batt LED | `LED_POWER_CRITICAL=LED_RED (= 3)` |
| Buzzer | `PIN_BUZZER=(0 + 25) = 25` |

**Behavior**

- User button on (0 + 6) = 6; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=false, activePullup=false
- Power/status LED on PIN_LED2 (= 24) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s; critical state moves to LED_POWER_CRITICAL LED_RED (= 3)
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on (0 + 25) = 25: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = false
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = (0 + 6) = 6
BUTTON_SENSE_TYPE = 0x5 = 5
HAS_BUTTON = 1
LED_BLUE = -1
LED_POWER = PIN_LED2 (= 24)
LED_POWER_CRITICAL = LED_RED (= 3)
LED_RED = PIN_LED1 (= 3)
LED_STATE_ON = 1
PIN_BUZZER = (0 + 25) = 25
PIN_LED1 = (0 + 3) = 3
PIN_LED2 = (0 + 24) = 24
PIN_LED3 = (0 + 28) = 28
```

</details>

### nrf52840/seeed_solar_node

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=D13 (= 13)` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| Touch button | `BUTTON_PIN_TOUCH=20` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(12)` |

**Behavior**

- User button on D13 (= 13); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=false
- Touch button on 20: 500ms hold=BACK
- Power/status LED on PIN_LED1(12) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = D13 (= 13)
BUTTON_PIN_TOUCH = 20
LED_BLUE = PIN_LED2 (= 11)
LED_GREEN = PIN_LED1 (= 12)
LED_STATE_ON = 1
PIN_LED1 = 12
PIN_LED2 = 11
```

</details>

### nrf52840/seeed_wio_tracker_L1

Platform `nrf52840` · 🟡 **DIFFERS: ADC_CTRL_ENABLED**

| Declared | Effective value |
|---|---|
| Cancel button | `CANCEL_BUTTON_PIN=D13 (= 13)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(11)` |
| Buzzer | `PIN_BUZZER=D12 (= 12)` |

**Behavior**

- Cancel button on D13 (= 13): click=CANCEL, 4s hold=SHUTDOWN
- Power/status LED on PIN_LED1(11) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on D12 (= 12): ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
CANCEL_BUTTON_ACTIVE_LOW = true
CANCEL_BUTTON_ACTIVE_PULLUP = false
CANCEL_BUTTON_PIN = D13 (= 13)
LED_BLUE = PIN_LED2 (= 12)
LED_GREEN = PIN_LED1 (= 11)
LED_STATE_ON = 1
PIN_BUZZER = D12 (= 12)
PIN_LED1 = 11
PIN_LED2 = 12
```

</details>

### nrf52840/seeed_wio_tracker_L1_eink

Platform `nrf52840` · 🟡 **DIFFERS: ADC_CTRL_ENABLED**

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=D13 (= 13)` |
| Cancel button | `CANCEL_BUTTON_PIN=D13 (= 13)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1(11)` |
| Buzzer | `PIN_BUZZER=D12 (= 12)` |

**Behavior**

- User button on D13 (= 13); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Cancel button on D13 (= 13): click=CANCEL, 4s hold=SHUTDOWN
- Power/status LED on PIN_LED1(11) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on D12 (= 12): ExternalNotification default (PWM, alert on message) + system beeps
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu) | button1 on TB_PRESS (= 29) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = D13 (= 13)
CANCEL_BUTTON_ACTIVE_LOW = true
CANCEL_BUTTON_ACTIVE_PULLUP = false
CANCEL_BUTTON_PIN = D13 (= 13)
LED_BLUE = PIN_LED2 (= 12)
LED_GREEN = PIN_LED1 (= 11)
LED_STATE_ON = 1
PIN_BUZZER = D12 (= 12)
PIN_EINK_EN = 14
PIN_LED1 = 11
PIN_LED2 = 12
```

</details>

### nrf52840/seeed_xiao_nrf52840_kit

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=D0` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1(LED_GREEN (= 13))` |

**Behavior**

- User button on D0; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1(LED_GREEN (= 13)) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE, LED_RED

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = LOW
BUTTON_PIN = D0
LED_BLUE = 12
LED_GREEN = 13
LED_RED = 11
LED_STATE_ON = 0
PIN_LED1 = LED_GREEN (= 13)
PIN_LED2 = LED_BLUE (= 12)
PIN_LED3 = LED_RED (= 11)
```

</details>

### nrf52840/t-echo

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=true` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2((0 + 18) = 18)` |
| Touch button | `BUTTON_PIN_TOUCH=PIN_BUTTON_TOUCH((0 + 11) = 11)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 14) = 14)` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=true
- Alt/back button on PIN_BUTTON2((0 + 18) = 18): click=ALT_PRESS, 500ms hold=ALT_LONG
- Touch button on PIN_BUTTON_TOUCH((0 + 11) = 11): momentary e-ink backlight while held (no BACK event)
- Power/status LED on PIN_LED1((0 + 14) = 14) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_GREEN, LED_RED
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu) | button1 on PIN_BUTTON_TOUCH (= 11) -> Down: backlight peek; LongPress: backlight latch on; ShortPress: backlight off

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = true
BUTTON_CLICK_MS = 400
BUTTON_TOUCH_ACTIVE_LOW = true
BUTTON_TOUCH_ACTIVE_PULLUP = true
BUTTON_TOUCH_MS = 200
LED_BLUE = PIN_LED1 (= 14)
LED_GREEN = PIN_LED2 (= 15)
LED_RED = PIN_LED3 (= 13)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 10) = 42
PIN_BUTTON2 = (0 + 18) = 18
PIN_BUTTON_TOUCH = (0 + 11) = 11
PIN_EINK_EN = (32 + 11) = 43
PIN_LED1 = (0 + 14) = 14
PIN_LED2 = (0 + 15) = 15
PIN_LED3 = (0 + 13) = 13
```

</details>

### nrf52840/t-echo-card

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| NeoPixel status (power/pair/notif) | `NEOPIXEL_STATUS_POWER_PIN=WS2812_DATA_1 (= 39)`<br>`NEOPIXEL_STATUS_PAIRING_PIN=WS2812_DATA_3 (= 28)`<br>`NEOPIXEL_STATUS_NOTIFICATION_PIN=WS2812_DATA_2 (= 44)` |
| Buzzer | `PIN_BUZZER=(32 + 6) = 38` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- NeoPixel power-status pixel on WS2812_DATA_1 (= 39) color 0x400000 = 4194304
- NeoPixel pairing-status pixel on WS2812_DATA_3 (= 28) color 0x000040 = 64
- NeoPixel notification pixel on WS2812_DATA_2 (= 44)
- Buzzer on (32 + 6) = 38: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BUTTON_CLICK_MS = 400
LED_STATE_ON = 1
NEOPIXEL_STATUS_NOTIFICATION_COLOR = 0x004000 = 16384
NEOPIXEL_STATUS_NOTIFICATION_PIN = WS2812_DATA_2 (= 44)
NEOPIXEL_STATUS_PAIRING_COLOR = 0x000040 = 64
NEOPIXEL_STATUS_PAIRING_PIN = WS2812_DATA_3 (= 28)
NEOPIXEL_STATUS_POWER_COLOR = 0x400000 = 4194304
NEOPIXEL_STATUS_POWER_PIN = WS2812_DATA_1 (= 39)
OLED_GEOMETRY_OVERRIDE = GEOMETRY_72_40
OLED_TINY
OLED_Y_OFFSET_PAGES = 3
PIN_BUTTON1 = (32 + 10) = 42
PIN_BUZZER = (32 + 6) = 38
```

</details>

### nrf52840/t-echo-lite

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((0 + 24) = 24)` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2((0 + 18) = 18)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((32 + 7) = 39)` |
| BLE LED | `BLE_LED=LED_BLUE (= 37)` |

**Behavior**

- User button on PIN_BUTTON1((0 + 24) = 24); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Alt/back button on PIN_BUTTON2((0 + 18) = 18): click=ALT_PRESS, 500ms hold=ALT_LONG
- Power/status LED on PIN_LED1((32 + 7) = 39) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- BLE LED on LED_BLUE (= 37) (on while BLE connected)
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_RED

<details><summary>All matched defines (raw)</summary>

```c
ADC_CTRL_ENABLED = HIGH
BLE_LED = LED_BLUE (= 37)
BUTTON_CLICK_MS = 400
LED_BLUE = PIN_LED2 (= 37)
LED_GREEN = PIN_LED1 (= 39)
LED_RED = PIN_LED3 (= 46)
LED_STATE_ON = 0
PIN_BUTTON1 = (0 + 24) = 24
PIN_BUTTON2 = (0 + 18) = 18
PIN_LED1 = (32 + 7) = 39
PIN_LED2 = (32 + 5) = 37
PIN_LED3 = (32 + 14) = 46
```

</details>

### nrf52840/t-echo-plus

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PIN_BUTTON1((32 + 10) = 42)` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=true` |
| Alt/Back button | `ALT_BUTTON_PIN=PIN_BUTTON2((0 + 18) = 18)` |
| Touch button | `BUTTON_PIN_TOUCH=PIN_BUTTON_TOUCH((0 + 11) = 11)` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 14) = 14)` |
| Buzzer | `PIN_BUZZER=(0 + 6) = 6` |

**Behavior**

- User button on PIN_BUTTON1((32 + 10) = 42); with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=true
- Alt/back button on PIN_BUTTON2((0 + 18) = 18): click=ALT_PRESS, 500ms hold=ALT_LONG
- Touch button on PIN_BUTTON_TOUCH((0 + 11) = 11): momentary e-ink backlight while held (no BACK event)
- Power/status LED on PIN_LED1((0 + 14) = 14) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_GREEN, LED_RED
- Buzzer on (0 + 6) = 6: ExternalNotification default (PWM, alert on message) + system beeps
- InkHUD (niche) UI build — TwoButton wiring replaces standard ButtonThread when InkHUD display mode is active: button0 on user BUTTON_PIN (settings-overridable) -> ShortPress: InkHUD shortpress (advance/select); LongPress: InkHUD longpress (menu) | button1 on PIN_BUTTON_TOUCH (= 11) -> Down: backlight peek; LongPress: backlight latch on; ShortPress: backlight off

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = true
BUTTON_CLICK_MS = 400
BUTTON_TOUCH_ACTIVE_LOW = true
BUTTON_TOUCH_ACTIVE_PULLUP = true
BUTTON_TOUCH_MS = 200
LED_BLUE = PIN_LED1 (= 14)
LED_GREEN = PIN_LED2 (= 15)
LED_RED = PIN_LED3 (= 13)
LED_STATE_ON = 0
PIN_BUTTON1 = (32 + 10) = 42
PIN_BUTTON2 = (0 + 18) = 18
PIN_BUTTON_TOUCH = (0 + 11) = 11
PIN_BUZZER = (0 + 6) = 6
PIN_EINK_EN = PIN_EINK_BL (= 43)
PIN_LED1 = (0 + 14) = 14
PIN_LED2 = (0 + 15) = 15
PIN_LED3 = (0 + 13) = 13
```

</details>

### nrf52840/t-impulse-plus

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| Touch button | `BUTTON_PIN_TOUCH=PIN_BUTTON_TOUCH(D18 (= 18))` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Notification LED | `LED_NOTIFICATION=D19 (= 19)` |

**Behavior**

- Touch button on PIN_BUTTON_TOUCH(D18 (= 18)): 500ms hold=BACK; haptic blip on touch + at long-press threshold
- Notification LED on D19 (= 19) (ExternalNotification default output, 1000ms on message)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_TOUCH_ACTIVE_LOW = true
BUTTON_TOUCH_ACTIVE_PULLUP = false
HAPTIC_FEEDBACK_PIN = LED_NOTIFICATION (= 19)
LED_NOTIFICATION = D19 (= 19)
LED_STATE_ON = 1
OLED_GEOMETRY_OVERRIDE = GEOMETRY_64_32
OLED_TINY
PIN_BUTTON_TOUCH = D18 (= 18)
```

</details>

### nrf52840/tracker-t1000-e

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(0 + 6) = 6` |
| Btn active-low | `BUTTON_ACTIVE_LOW=false` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1 (= 24)` |
| Buzzer | `PIN_BUZZER=(0 + 25) = 25`<br>`BUZZER_EN_PIN=(32 + 5) = 37` |

**Behavior**

- User button on (0 + 6) = 6; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=false, activePullup=false
- Power/status LED on PIN_LED1 (= 24) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on (0 + 25) = 25: ExternalNotification default (PWM, alert on message) + system beeps
- Buzzer enable rail on (32 + 5) = 37 (driven low for sleep)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = false
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = (0 + 6) = 6
BUTTON_SENSE_TYPE = 0x5 = 5
BUZZER_EN_PIN = (32 + 5) = 37
LED_BLUE = -1
LED_POWER = PIN_LED1 (= 24)
LED_STATE_ON = 1
PIN_BUZZER = (0 + 25) = 25
PIN_LED1 = (0 + 24) = 24
```

</details>

### nrf52840/wio-sdk-wm1110

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(0 + 23) = 23` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 13) = 13)` |

**Behavior**

- User button on (0 + 23) = 23; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((0 + 13) = 13) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = (0 + 23) = 23
LED_BLUE = PIN_LED2 (= 14)
LED_GREEN = PIN_LED1 (= 13)
LED_STATE_ON = 1
PIN_LED1 = (0 + 13) = 13
PIN_LED2 = (0 + 14) = 14
```

</details>

### nrf52840/wio-t1000-s

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(0 + 6) = 6` |
| Btn active-low | `BUTTON_ACTIVE_LOW=false` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1 (= 24)` |
| Buzzer | `PIN_BUZZER=(0 + 25) = 25`<br>`BUZZER_EN_PIN=(32 + 5) = 37` |

**Behavior**

- User button on (0 + 6) = 6; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=false, activePullup=false
- Power/status LED on PIN_LED1 (= 24) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): LED_BLUE
- Buzzer on (0 + 25) = 25: ExternalNotification default (PWM, alert on message) + system beeps
- Buzzer enable rail on (32 + 5) = 37 (driven low for sleep)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = false
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = (0 + 6) = 6
BUTTON_SENSE_TYPE = INPUT_SENSE_HIGH
BUZZER_EN_PIN = (32 + 5) = 37
LED_BLUE = -1
LED_POWER = PIN_LED1 (= 24)
LED_STATE_ON = 1
PIN_BUZZER = (0 + 25) = 25
PIN_LED1 = (0 + 24) = 24
```

</details>

### nrf52840/wio-tracker-wm1110

Platform `nrf52840`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=(32 + 2) = 34` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PIN_LED1((0 + 6) = 6)` |

**Behavior**

- User button on (32 + 2) = 34; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED1((0 + 6) = 6) (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = (32 + 2) = 34
LED_GREEN = PIN_LED1 (= 6)
LED_STATE_ON = 0
PIN_LED1 = (0 + 6) = 6
```

</details>

### rp2040/challenger_2040_lora

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=0xFFFFFFFF` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=0xFFFFFFFF` |

**Behavior**

- User button on 0xFFFFFFFF; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 0xFFFFFFFF
EXT_NOTIFY_OUT = 0xFFFFFFFF
```

</details>

### rp2040/ec_catsniffer

Platform `rp2040`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=27` |

**Behavior**

- Power/status LED on 27 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_POWER = 27
```

</details>

### rp2040/feather_rp2040_rfm95

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=7` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 7; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 7
EXT_NOTIFY_OUT = 22
LED_POWER = PIN_LED
```

</details>

### rp2040/nibble_rp2040

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=-1` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=1` |

**Behavior**

- User button on -1; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on 1 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = -1
LED_POWER = 1
```

</details>

### rp2040/rak11310

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=9` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED1` |

**Behavior**

- User button on 9; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 9
LED_POWER = PIN_LED1
ledOff = (pin) pinMode(pin, INPUT)
```

</details>

### rp2040/rp2040-lora

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=-1` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on -1; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = -1
EXT_NOTIFY_OUT = 22
LED_POWER = PIN_LED
```

</details>

### rp2040/rpipico

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=17` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 17; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 17
EXT_NOTIFY_OUT = 22
LED_POWER = PIN_LED
```

</details>

### rp2040/rpipico-slowclock

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=18` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 18; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 18
EXT_NOTIFY_OUT = 22
LED_POWER = PIN_LED
RESET_OLED = 13
```

</details>

### rp2040/rpipicow

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=17` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 17; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 17
EXT_NOTIFY_OUT = 22
LED_POWER = PIN_LED
```

</details>

### rp2040/senselora_rp2040

Platform `rp2040`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=2` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED1` |

**Behavior**

- User button on 2; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PIN_LED1 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = 2
LED_POWER = PIN_LED1
ledOff = (pin) pinMode(pin, INPUT)
```

</details>

### rp2350/rpipico2

Platform `rp2350`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=17` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| Power/status LED | `LED_POWER=PIN_LED` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 17; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)
- Power/status LED on PIN_LED (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 17
EXT_NOTIFY_OUT = 22
LED_POWER = PIN_LED
```

</details>

### rp2350/rpipico2w

Platform `rp2350`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=17` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1 (default)` |
| EXT_NOTIFY_OUT | `EXT_NOTIFY_OUT=22` |

**Behavior**

- User button on 17; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default)

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_PIN = 17
EXT_NOTIFY_OUT = 22
```

</details>

### stm32/CDEBYTE_E77-MBL

Platform `stm32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PB4` |

**Behavior**

- Power/status LED on PB4 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_POWER = PB4
LED_STATE_ON = 1
```

</details>

### stm32/milesight_gs301

Platform `stm32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PC13` |
| Btn active-low | `BUTTON_ACTIVE_LOW=true` |
| Btn pullup | `BUTTON_ACTIVE_PULLUP=false` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PIN_LED1` |
| Buzzer | `PIN_BUZZER=PA6` |

**Behavior**

- User button on PC13; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true, activePullup=false
- Power/status LED on PIN_LED1 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s
- Defined but not consumed by firmware LED logic (candidate dead defines): USER_LED
- Buzzer on PA6: ExternalNotification default (PWM, alert on message) + system beeps

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_ACTIVE_LOW = true
BUTTON_ACTIVE_PULLUP = false
BUTTON_PIN = PC13
LED_POWER = PIN_LED1
LED_STATE_ON = 1
PIN_BUZZER = PA6
PIN_LED1 = PA0
PIN_LED2 = PA0
USER_LED = PIN_LED2
```

</details>

### stm32/rak3172

Platform `stm32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PA0` |

**Behavior**

- Power/status LED on PA0 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_POWER = PA0
LED_STATE_ON = 1
```

</details>

### stm32/russell

Platform `stm32`

| Declared | Effective value |
|---|---|
| User button (BUTTON_PIN) | `BUTTON_PIN=PH3` |
| Btn pullup | `BUTTON_NEED_PULLUP` |
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=1` |
| Power/status LED | `LED_POWER=PA0` |

**Behavior**

- User button on PH3; with screen: click=USER_PRESS (cycle UI), 500ms hold=SELECT, 3.9s hold=SHUTDOWN; no screen: click=USER_PRESS, 3.9s hold=SHUTDOWN, double=SEND_PING, triple=GPS_TOGGLE; activeLow=true(default), activePullup=true(default); internal pullup enabled (BUTTON_NEED_PULLUP)
- Power/status LED on PA0 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
BUTTON_NEED_PULLUP
BUTTON_PIN = PH3
LED_POWER = PA0
LED_STATE_ON = 1
```

</details>

### stm32/wio-e5

Platform `stm32`

| Declared | Effective value |
|---|---|
| LED polarity (LED_STATE_ON) | `LED_STATE_ON=0` |
| Power/status LED | `LED_POWER=PB5` |

**Behavior**

- Power/status LED on PB5 (StatusLEDModule): discharging=1ms blip/s heartbeat, charging=1s toggle blink, charged=solid, critical(<5%)=250ms fast-blink burst every 30s

<details><summary>All matched defines (raw)</summary>

```c
LED_POWER = PB5
LED_STATE_ON = 0
```

</details>
