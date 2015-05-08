# zah2
КлассMainClass.java :

package lab3RaspredSys;

importjava.util.Scanner;
importjava.util.concurrent.ForkJoinPool;
importjava.util.concurrent.RecursiveTask;

public class MainClass {

	static final ForkJoinPoolmainPool = new ForkJoinPool();

	public static void main(String[] args) {
		Scanner scn = new Scanner(System.in);
		System.out.println("Введите мд5: ");
		String md5 = scn.nextLine();
		scn.close();
		longstartTime = System.currentTimeMillis();
		RecursiveTask<Long> key = new FindKey(md5, 97, 122);
		longfk = mainPool.invoke(key);

		String h = String.valueOf(fk);
		String[] parts = h.split("6");
		String result = "";
		for (inti = 0; i<parts.length; i++) {
			int r = Integer.valueOf(parts[i]);
			result = result + String.valueOf((char) r);
		}
		System.out.println(result);
		longtimeSpent = System.currentTimeMillis() - startTime;
		System.out.println("Поисквыполнялся " + timeSpent + " миллисекунд");
	}

}

КлассFindKey.java :

package lab3RaspredSys;

import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.concurrent.RecursiveAction;
import java.util.concurrent.RecursiveTask;

public class FindKey extends RecursiveTask<Long> {

Long sd = new Long(0);
final String md5;
final int lo;
final int hi;
static int p = 0;

FindKey(String md5, int lo, int hi) {
this.md5 = md5;
this.lo = lo;
this.hi = hi;
}

@Override
protected Long compute() {
if (hi - lo < 2) {
for (int i = lo; i < hi; i++) {
if (md5Custom(String.valueOf((char) i)).equals(md5)) {
sd = (long) i;
p = 1;
break;
}
}
if (p == 0) {
for (int i = lo; i < hi; i++) {
for (int j = 97; j < 122; j++) {
if (md5Custom(
(String.valueOf((char) i))
+ String.valueOf((char) j)).equals(md5)) {
String g = String.valueOf(i) + "6"
+ String.valueOf(j);
sd = (long) Long.parseLong(g);
p = 1;
break;
}
}
}
}
if (p == 0) {
for (int i = lo; i < hi; i++) {
for (int j = 97; j < 122; j++) {
for (int k = 97; k < 122; k++) {
if (md5Custom(
(String.valueOf((char) i))
+ String.valueOf((char) j)
+ String.valueOf((char) k)).equals(
md5)) {
String g = String.valueOf(i) + "6"
+ String.valueOf(j) + "6"
+ String.valueOf(k);
sd = (long) Long.parseLong(g);
p = 1;
break;
}
}
}
}
}
if (p == 0) {
for (int i = lo; i < hi; i++) {
for (int j = 97; j < 122; j++) {
for (int k = 97; k < 122; k++) {
for (int l = 97; l < 122; l++) {
if (md5Custom(
(String.valueOf((char) i))
+ String.valueOf((char) j)
+ String.valueOf((char) k)
+ String.valueOf((char) l))
.equals(md5)) {
String g = String.valueOf(i) + "6"
+ String.valueOf(j) + "6"
+ String.valueOf(k) + "6"
+ String.valueOf(l);
sd = (long) Long.parseLong(g);
p = 1;
break;
}
}
}
}
}
}
return sd;
} else {
int mid = (lo + hi) >>> 1;
RecursiveTask<Long> left = new FindKey(md5, lo, mid);
RecursiveTask<Long> right = new FindKey(md5, mid, hi);
right.fork();
return Math.max(left.invoke(), right.join());
}
}

public static String md5Custom(String st) {
MessageDigest messageDigest = null;
byte[] digest = new byte[0];

try {
messageDigest = MessageDigest.getInstance("MD5");
messageDigest.reset();
messageDigest.update(st.getBytes());
digest = messageDigest.digest();
} catch (NoSuchAlgorithmException e) {
e.printStackTrace();
}

BigInteger bigInt = new BigInteger(1, digest);
String md5Hex = bigInt.toString(16);

while (md5Hex.length() < 32) {
md5Hex = "0" + md5Hex;
}

return md5Hex;
}

}
