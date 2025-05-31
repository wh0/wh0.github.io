---
date: Fri May 30 2025 22:48:11 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Why Firefox named that file 3870112724rsegmnoittet-es.sqlite
previous_teaser: A font that swaps ")" and "}"
previous_first: false
---
I just need to write this down before I have to figure it out a third time.

In your Firefox profile directory, there are these indexedDB-related files with scrambled up names.
For example, there's `~/.mozilla/firefox/(random stuff).default-release/storage/permanent/chrome/idb/3870112724rsegmnoittet-es.sqlite`.

The numbers at the beginning are a hash of the database name, and
after that there's characters from the beginning and end of the database name, interleaved.

```
3870112724rsegmnoittet-es.sqlite

3870112724  rsegmnoittet-es  .sqlite
|--hash--|  |-interleaved-|

-------------->
r e m o t e - s
rsegmnoittet-es
 s g n i t t e
 <------------

remote-settings
```

Here are some filenames and the database names that they were scrambled from.

| Filename                               | Database name                    |
|:---------------------------------------|:---------------------------------|
| 2728594770keeryovtasl-.sqlite          | keyval-store                     |
| 2057579554yatt-asdeartveimc-erw.sqlite | yt-serviceworker-metadata        |
| 2171031483YattIedMb.sqlite             | YtIdbMeta                        |
| 3211250388sbwdpsunsohintoatciif.sqlite | swpushnotificationsdb            |
| 3951282036IBmDaeghecCa.sqlite          | ImageCacheDB                     |
| 953658429glmaaviyle-ks-w.sqlite        | gmail-sw-keyval                  |
| 1573635177setnoirlafgfeo..bw.sqlite    | storage.bw.offline               |
| 4041069256GsoDoegvlierD.sqlite         | GoogleDriveDs                    |
| 2366965780vbsdc-obdeew-.sqlite         | vscode-web-db                    |
| 4277777170vlsacboodleg--wbedb--.sqlite | vscode-web-state-db-global       |
| 2397490645vwsocdondiew--wyetbp-.sqlite | vscode-web-state-db-empty-window |
| 1460062179wswgCiofn.sqlite             | wwConfigs                        |
| 276181416SyWr.tTeemle                  | SW.Telemetry                     |
| 1385285754oednsipl_f1fdos_.sqlite      | odsp_1ds_offline                 |
| 3215510899Sgiitfen.oC.sqlite           | Site.Config                      |
| 3217723701OBDDS_P.sqlite               | ODSP_DB                          |
| 340685107feisraebbaatsaed--isn.sqlite  | firebase-installations-database  |
| 2926346687feisraebbaatsaed--hte.sqlite | firebase-heartbeat-database      |
| 3619119340leogcaarlof.sqlite           | localforage                      |
| 3044360959pp.sqlite                    | pp                               |
| 1680441260lsog.sqlite                  | logs                             |
| 2843657506mkadtsr-itxc-are             | matrix-react-sdk                 |
| 4197078560wnooriktbaorxi-pex           | workbox-expiration               |
| 1671402671ueBglaorcokt0SCeahc.sqlite   | uBlock0CacheStorage              |
| 3647222921wleabcEoxlt-eengsairo.sqlite | webExtensions-storage-local      |
| 1451318868ntouromlalnodry--epcr.sqlite | normandy-preference-rollout      |
| 1657114595AmcateirvtiSty.sqlite        | ActivityStream                   |
| 2823318777ntouromlalnodry--naod.sqlite | normandy-addon-rollout           |
| 2918063365piupsah.sqlite               | pushapi                          |
| 3561288849sdhlie.sqlite                | shield                           |

The empty string hashes to 0, and a database with empty name would make `0.sqlite`.
Apparently there's no rule against using an empty string as an indexedDB name.

The developers who added this name scrambling logic didn't leave a written record about the rationale.
The scrambling logic [appeared in a second version of a patch](https://bugzilla.mozilla.org/show_bug.cgi?id=591516#c7),
with the team noting,
"We did this review over phone," and
"Ok, this fixes all the stuff we found in review."

Yes I did just end a "Why ..." article without answering why.
