---
description: "Das UserMenu von Midnight Commander lässt sich einfach erweitern, um Dateien mit AGE zu verschlüsseln."
tags:
- Der Software-Engineer
- Der Nerd
- CLI
- Encryption
---

# AGE + GZIP in Midnight Commander integrieren

Ein Code-Snippet für Midnight Commander um schnelle Kompression und Verschlüsselung mittels GZIP und AGE ins Kontextmenü zu integrieren.

```shell
6	Comp. + encr. tagged files into separate archives (PIGZ+AGE)
	recp="<insertPublicKey>"
	for i in %t
	do
		name=`basename "$i"`
		tar --use-compress-program="pigz --best --recursive" -cvf $name.tar.gz $i && \
		age -r $recp $name.tar.gz > $name.tar.gz.age && \
		rm $name.tar.gz && \
		echo "$name.tar.gz.age" created.
	done
```