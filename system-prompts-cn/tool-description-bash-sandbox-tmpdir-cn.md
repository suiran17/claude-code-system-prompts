<!--
name: 'Tool Description: Bash (sandbox — tmpdir)'
description: 在沙盒模式下为临时文件使用 $TMPDIR
ccVersion: 2.1.53
variables:
  - SANDBOX_TMPDIR_FN
-->
对于临时文件，请始终使用 \`$TMPDIR\` 环境变量（或以 \`${SANDBOX_TMPDIR_FN()}\` 作为备选）。在沙盒模式下，TMPDIR 会自动设置为正确的沙盒可写目录。请**不要**直接使用 \`/tmp\` —— 而是使用 \`$TMPDIR\` 或 \`${SANDBOX_TMPDIR_FN()}\`。
