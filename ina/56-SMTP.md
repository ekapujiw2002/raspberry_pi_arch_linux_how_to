# SMTP Relay Server
1. Install paket **ssmtp** dengan `sudo pacman -S --needed ssmtp`
2. Edit file konfigurasinya di **/etc/ssmtp/ssmtp.conf** dan isi sesuai seting server SMTP yang akan dipergunakan. Untuk contoh ini maka dipergunakan server GMail dengan username xxx dan password 1234 :
  ```
  # The user that gets all the mails (UID < 1000, usually the admin)
  root=xxx@gmail.com
  
  # The mail server (where the mail is sent to), both port 465 or 587 should be acceptable
  # See also http://mail.google.com/support/bin/answer.py?answer=78799
  mailhub=smtp.gmail.com:587
  
  # The address where the mail appears to come from for user authentication.
  rewriteDomain=gmail.com
  
  # The full hostname
  hostname=localhost
  
  # Use SSL/TLS before starting negotiation
  UseTLS=Yes
  UseSTARTTLS=Yes
  
  # Username/Password
  AuthUser=xxx
  AuthPass=1234
  
  # Email 'From header's can override the default domain?
  FromLineOverride=yes
  ```
3. Tes email dengan kirim email misal ke **ekapujiw2002@gmail.com** dengan perintah : `echo test | mail -v -s "testing ssmtp setup" ekapujiw2002@gmail.com` sehingga akan menghasilkan respon kalau sukses sebagai berikut :
  ```
  [raspi@raspiku ~]$ echo test | mail -v -s "testing ssmtp setup" ekapujiw2002@gmail.com
  LOAD 15 bytes <#@ /etc/mail.rc>
  LOAD 41 bytes <#@ Configuration file for Mail(1) v14.8.5>
  LOAD 33 bytes <# S-nail(1): v14.8.5 / 2015-09-05>
  LOAD 0 bytes <>
  LOAD 78 bytes <## The standard POSIX 2008/Cor 1-2013 mandates the following initial settings:>
  LOAD 78 bytes <# (Keep in sync: ./main.c:_startup(), ./nail.rc, ./nail.1:"Initial settings"!)>
  LOAD 67 bytes <# [a]   noallnet, noappend, asksub, noaskbcc, noaskcc, noautoprint,>
  LOAD 57 bytes <# [b-e] nobang, nocmd, nocrt, nodebug, nodot, escape="~",>
  LOAD 65 bytes <# [f-i] noflipr, nofolder, header, nohold, noignore, noignoreeof,>
  LOAD 49 bytes <# [j-o] nokeep, nokeepsave, nometoo, nooutfolder,>
  LOAD 47 bytes <# [p-r] nopage, prompt="? ", noquiet, norecord,>
  LOAD 51 bytes <# [s]   save, nosendwait, noshowto, nosign, noSign,>
  LOAD 20 bytes <# [t-z] toplines="5">
  LOAD 8 bytes <# Notes:>
  LOAD 52 bytes <# - no*onehop* doesn't exist in this implementation.>
  LOAD 78 bytes <#   (To pass options through to an MTA, either add them after a "--" separator>
  LOAD 73 bytes <#   on the command line or by setting the *sendmail-arguments* variable.)>
  LOAD 65 bytes <# - *prompt* is "\\& " by default, which will act POSIX-compliant>
  LOAD 41 bytes <#   unless the user would set *bsdcompat*>
  LOAD 0 bytes <>
  LOAD 71 bytes <## The remaining content adjusts the standard-imposed default settings.>
  LOAD 78 bytes <# Note that some of the following flags are specific to S-nail(1) and may thus>
  LOAD 50 bytes <# not work with other Mail(1) / mailx(1) programs.>
  LOAD 77 bytes <# Entries are marked [OPTION] if their availability is compile-time dependent>
  LOAD 0 bytes <>
  LOAD 12 bytes <## Variables>
  LOAD 0 bytes <>
  LOAD 62 bytes <# If threaded mode is activated, automatically collapse thread>
  LOAD 16 bytes <set autocollapse>
  LOAD 0 bytes <>
  LOAD 35 bytes <# Enter threaded mode automatically>
  LOAD 20 bytes <#set autosort=thread>
  LOAD 0 bytes <>
  LOAD 64 bytes <# Append rather than prepend when writing to mbox automatically.>
  LOAD 61 bytes <# This has no effect unless *hold* is unset (it is set below)>
  LOAD 10 bytes <set append>
  LOAD 0 bytes <>
  LOAD 28 bytes <# Ask for a message subject.>
  LOAD 7 bytes <set ask>
  LOAD 0 bytes <>
  LOAD 77 bytes <# *bsdannounce* prints a header summary on folder change and thus complements>
  LOAD 75 bytes <# *header* on a per-folder basis (it is meaningless unless *header* is set)>
  LOAD 15 bytes <set bsdannounce>
  LOAD 0 bytes <>
  LOAD 59 bytes <# Uncomment this in order to get coloured output in $PAGER.>
  LOAD 74 bytes <# (Coloured output is only used if $TERM is either found in *colour-terms*>
  LOAD 33 bytes <# or includes the string "color")>
  LOAD 17 bytes <#set colour-pager>
  LOAD 0 bytes <>
  LOAD 48 bytes <# Assume a CRT-like terminal and invoke a $PAGER>
  LOAD 7 bytes <set crt>
  LOAD 0 bytes <>
  LOAD 39 bytes <# Define date display in header summary>
  LOAD 63 bytes <#set datefield="%R %m-%d" datefield-markout-older="   %g-%m-%d">
  LOAD 0 bytes <>
  LOAD 70 bytes <# When composing messages a line consisting of `.' finalizes a message>
  LOAD 7 bytes <set dot>
  LOAD 0 bytes <>
  LOAD 65 bytes <# Immediately start $EDITOR (or $VISUAL) when composing a message>
  LOAD 14 bytes <#set editalong>
  LOAD 0 bytes <>
  LOAD 68 bytes <# Startup into interactive mode even if the (given) mailbox is empty>
  LOAD 15 bytes <#set emptystart>
  LOAD 0 bytes <>
  LOAD 78 bytes <# When replying to or forwarding a message the comment and name parts of email>
  LOAD 52 bytes <# addresses are removed unless this variable is set.>
  LOAD 14 bytes <#set fullnames>
  LOAD 0 bytes <>
  LOAD 64 bytes <# [OPTION] Add more entries to the history as is done by default>
  LOAD 17 bytes <set history-gabby>
  LOAD 0 bytes <>
  LOAD 62 bytes <# Do not forward to mbox by default since this is likely to be>
  LOAD 54 bytes <# irritating for most users today; also see *keepsave*>
  LOAD 8 bytes <set hold>
  LOAD 0 bytes <>
  LOAD 72 bytes <# Quote the original message in replies by "> " as usual on the Internet>
  LOAD 21 bytes <set indentprefix="> ">
  LOAD 0 bytes <>
  LOAD 39 bytes <# Mark messages that have been answered>
  LOAD 16 bytes <set markanswered>
  LOAD 0 bytes <>
  LOAD 67 bytes <# Try to circumvent false or missing MIME Content-Type descriptions>
  LOAD 71 bytes <# (Can be set to values for extended behaviour, please see the manual.)>
  LOAD 25 bytes <set mime-counter-evidence>
  LOAD 0 bytes <>
  LOAD 78 bytes <# Control loading of mime.types(5) file: the value may be a combination of the>
  LOAD 79 bytes <# letters "s" and "u": if "u" is seen ~/.mime.types will be loaded if possible;>
  LOAD 77 bytes <# "s" adds /etc/mime.types, if available; setting this without any value uses>
  LOAD 69 bytes <# only a set of builtin mimetypes; the default behaviour equals "us".>
  LOAD 79 bytes <# An extended syntax that allows loading of other, specified files is available>
  LOAD 66 bytes <# if the value contains an equal sign "=", see the manual for more>
  LOAD 27 bytes <#set mimetypes-load-control>
  LOAD 0 bytes <>
  LOAD 35 bytes <# Do not remove empty mail folders.>
  LOAD 75 bytes <# This may be relevant for privacy since other users could otherwise create>
  LOAD 33 bytes <# them with different permissions>
  LOAD 8 bytes <set keep>
  LOAD 0 bytes <>
  LOAD 74 bytes <# Do not move `save'd or `write'n message to mbox by default since this is>
  LOAD 63 bytes <# likely to be irritating for most users today; also see *hold*>
  LOAD 12 bytes <set keepsave>
  LOAD 0 bytes <>
  LOAD 78 bytes <# When writing mailbox files we strip Content-Length: and Lines: header fields>
  LOAD 72 bytes <# from edited / changed messages, because S-nail doesn't deal with these>
  LOAD 77 bytes <# (non-standard) fields -- and since other MUAs may rely on their content, if>
  LOAD 78 bytes <# present, it seems more useful to strip them than to keep them, now that they>
  LOAD 54 bytes <# became invalid; set this to include them nonetheless>
  LOAD 24 bytes <#set keep-content-length>
  LOAD 0 bytes <>
  LOAD 46 bytes <# A nice prompt for ISO 6429/ECMA-48 terminals>
  LOAD 42 bytes <#set prompt="\033[31m?\?[\$ \@]\& \033[0m">
  LOAD 0 bytes <>
  LOAD 66 bytes <# Automatically quote the text of the message that is responded to>
  LOAD 9 bytes <set quote>
  LOAD 0 bytes <>
  LOAD 76 bytes <# On group replies, specify only the sender of the original mail in  To: and>
  LOAD 76 bytes <# mention it's other recipients in the secondary Cc: instead of placing them>
  LOAD 21 bytes <# all together in To:>
  LOAD 20 bytes <set recipients-in-cc>
  LOAD 0 bytes <>
  LOAD 71 bytes <# When responding to a message, try to answer in the same character set>
  LOAD 26 bytes <#set reply-in-same-charset>
  LOAD 0 bytes <>
  LOAD 77 bytes <# [OPTION] Outgoing messages are sent in UTF-8 if possible, otherwise LATIN1.>
  LOAD 74 bytes <# Note: it is highly advisable to read the section "Character sets" of the>
  LOAD 77 bytes <# manual in order to understand all the possibilities that exist to fine-tune>
  LOAD 74 bytes <# charset usage (variables also of interest: *ttycharset*, *charset-8bit*,>
  LOAD 74 bytes <# *sendcharsets-else-ttycharset*; and of course we inherit the $LC_CTYPE />
  LOAD 60 bytes <# $LC_ALL / $LANG environment variables and react upon them)>
  LOAD 33 bytes <set sendcharsets=utf-8,iso-8859-1>
  LOAD 0 bytes <>
  LOAD 76 bytes <# When sending a message wait until the MTA (including the builtin SMTP one)>
  LOAD 78 bytes <# exits before accepting further commands.  Only with this variable set errors>
  LOAD 43 bytes <# reported by the MTA will be recognizable!>
  LOAD 13 bytes <#set sendwait>
  LOAD 0 bytes <>
  LOAD 73 bytes <# Display real sender names in header summaries instead of only addresses>
  LOAD 12 bytes <set showname>
  LOAD 0 bytes <>
  LOAD 74 bytes <# Show recipients of messages sent by the user himself in header summaries>
  LOAD 10 bytes <set showto>
  LOAD 0 bytes <>
  LOAD 11 bytes <## Commands>
  LOAD 0 bytes <>
  LOAD 68 bytes <# Only include these selected header fields when forwarding messages>
  LOAD 30 bytes <fwdretain subject date from to>
  LOAD 0 bytes <>
  LOAD 64 bytes <# Only include the selected header fields when printing messages>
  LOAD 67 bytes <retain date from to cc subject message-id mail-followup-to reply-to>
  LOAD 0 bytes <>
  LOAD 33 bytes <## Some pipe-TYPE/SUBTYPE entries>
  LOAD 0 bytes <>
  LOAD 42 bytes <# HTML as text, inline display via lynx(1)>
  LOAD 28 bytes <#if $features !@ HTML-FILTER>
  LOAD 54 bytes <#   set pipe-text/html="lynx -stdin -dump -force_html">
  LOAD 6 bytes <#endif>
  LOAD 0 bytes <>
  LOAD 47 bytes <# PDF display, asynchronous display via xpdf(1)>
  LOAD 292 bytes <#set pipe-application/pdf="@&set -C;#   : > \"${TMPDIR}/${NAIL_FILENAME_GENERATED}\";#   trap \"rm -f \\\"${TMPDIR}/${NAIL_FILENAME_GENERATED}\\\"\" #      EXIT INT QUIT PIPE TERM;#   set +C;#   cat > \"${TMPDIR}/${NAIL_FILENAME_GENERATED}\";#   xpdf \"${TMPDIR}/${NAIL_FILENAME_GENERATED}\"">
  LOAD 0 bytes <>
  LOAD 11 bytes <# s-it-mode>
  [<-] 220 smtp.gmail.com ESMTP wo3sm21564421pab.25 - gsmtp
  [->] EHLO localhost
  [<-] 250 SMTPUTF8
  [->] STARTTLS
  [<-] 220 2.0.0 Ready to start TLS
  [->] EHLO localhost
  [<-] 250 SMTPUTF8
  [->] AUTH LOGIN
  [<-] 334 VXNlcm5hbWU6
  [->] dHJhcGVyd3dmMjAxNQ==
  [<-] 334 UGFzc3dvcmQ6
  [<-] 235 2.7.0 Accepted
  [->] MAIL FROM:<raspi@gmail.com>
  [<-] 250 2.1.0 OK wo3sm21564421pab.25 - gsmtp
  [->] RCPT TO:<ekapujiw2002@gmail.com>
  [<-] 250 2.1.5 OK wo3sm21564421pab.25 - gsmtp
  [->] DATA
  [<-] 354  Go ahead wo3sm21564421pab.25 - gsmtp
  [->] Received: by localhost (sSMTP sendmail emulation); Sat, 05 Dec 2015 15:06:56 +0700
  [->] From: raspi@gmail.com
  [->] Date: Sat, 05 Dec 2015 15:06:56 +0700
  [->] To: ekapujiw2002@gmail.com
  [->] Subject: testing ssmtp setup
  [->] User-Agent: mail v14.8.5
  [->]
  [->] test
  [->] .
  [<-] 250 2.0.0 OK 1449302828 wo3sm21564421pab.25 - gsmtp
  [->] QUIT
  [<-] 221 2.0.0 closing connection wo3sm21564421pab.25 - gsmtp
  [raspi@raspiku ~]$
  ```
4. Kalau tidak ingin panjang seperti itu, maka hilangkan opsi **-v** di perintahnya
5. Perintah tanpa **-v** akan langsng kembali ke konsol, jika ingin menunggu status hasil pengiriman, maka pergunakan opsi **-Ssendwait**
6. Untuk mengirim email dengan attachment maka gunakan opsi **a**. Opsi ini bisa dipergunakan untuk mengirim lebih dari 1 attachment sekaligus seperti contoh berikut ini : `echo test lagi gan pada `date` dengan attachment | mail -a tes.log -a slime.swf -s "testing ssmtp setup" -Ssendwait ekapujiw2002@gmail.com`

Referensi :
- https://wiki.archlinux.org/index.php/SSMTP
