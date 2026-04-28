# Sankar-vegitable-storee
import React, { useMemo, useState } from "react";
import { motion } from "framer-motion";
import {
  ShoppingCart,
  Search,
  Leaf,
  Truck,
  ShieldCheck,
  Star,
  Minus,
  Plus,
  Trash2,
  ChevronRight,
  Sparkles,
  BadgePercent,
} from "lucide-react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Badge } from "@/components/ui/badge";
import { Separator } from "@/components/ui/separator";

const products = [
  { id: 1, name: "টমেটো", category: "পাতাজাতীয়", price: 30, unit: "কেজি", rating: 4.8, fresh: "আজই তোলা", image: "https://images.unsplash.com/photo-1546094096-0df4bcaaa337" },
  { id: 2, name: "আলু", category: "মূলজাতীয়", price: 25, unit: "কেজি", rating: 4.7, fresh: "ফার্ম ফ্রেশ", image: "https://images.unsplash.com/photo-1582515073490-dc1a3e0c7b1d" },
  { id: 3, name: "পেঁয়াজ", category: "মূলজাতীয়", price: 35, unit: "কেজি", rating: 4.6, fresh: "নতুন স্টক", image: "https://images.unsplash.com/photo-1587049633312-d628ae50a8ae" },
  { id: 4, name: "গাজর", category: "মূলজাতীয়", price: 40, unit: "কেজি", rating: 4.9, fresh: "অর্গানিক", image: "https://images.unsplash.com/photo-1582515073490-dc1a3e0c7b1d" },
  { id: 5, name: "শসা", category: "পাতাজাতীয়", price: 30, unit: "কেজি", rating: 4.5, fresh: "কোল্ড স্টোরেজ", image: "https://images.unsplash.com/photo-1567306226416-28f0efdc88ce" },
  { id: 6, name: "বেগুন", category: "ফলজাতীয়", price: 35, unit: "কেজি", rating: 4.6, fresh: "প্রিমিয়াম", image: "https://images.unsplash.com/photo-1601004890684-d8cbf643f5f2" }
];

const categories = ["সব", "মূলজাতীয়", "পাতাজাতীয়", "ফলজাতীয়", "ফুল/কপি", "মসলা"];

export default function VegetableShopWebsite() {
  const [query, setQuery] = useState("");
  const [category, setCategory] = useState("সব");
  const [cart, setCart] = useState([]);

  const filteredProducts = useMemo(() => {
    return products.filter((p) => {
      const matchesCategory = category === "সব" || p.category === category;
      const matchesQuery = p.name.toLowerCase().includes(query.toLowerCase());
      return matchesCategory && matchesQuery;
    });
  }, [query, category]);

  const addToCart = (product) => {
    setCart((prev) => {
      const found = prev.find((item) => item.id === product.id);
      if (found) {
        return prev.map((item) =>
          item.id === product.id ? { ...item, qty: item.qty + 1 } : item
        );
      }
      return [...prev, { ...product, qty: 1 }];
    });
  };

  const updateQty = (id, delta) => {
    setCart((prev) =>
      prev
        .map((item) => (item.id === id ? { ...item, qty: item.qty + delta } : item))
        .filter((item) => item.qty > 0)
    );
  };

  const removeItem = (id) => setCart((prev) => prev.filter((item) => item.id !== id));

  const total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);
  const deliveryFee = total > 499 || total === 0 ? 0 : 40;
  const grandTotal = total + deliveryFee;

  return (
    <div className="min-h-screen bg-gradient-to-b from-emerald-50 via-white to-lime-50 text-slate-900">
      <header className="sticky top-0 z-20 border-b bg-white/90 backdrop-blur">
        <div className="mx-auto flex max-w-7xl items-center justify-between gap-3 px-4 py-4">
          <div className="flex items-center gap-3">
            <div className="flex h-11 w-11 items-center justify-center rounded-2xl bg-emerald-600 text-white shadow-sm">
              <Leaf className="h-6 w-6" />
            </div>
            <div>
              <h1 className="text-xl font-bold leading-tight">Sankar Vegetable Store</h1>
              <p className="text-sm text-slate-500">তাজা সবজি, দ্রুত ডেলিভারি</p>
            </div>
          </div>
          <div className="flex items-center gap-2 rounded-full border bg-slate-50 px-3 py-2 text-sm font-medium">
            <ShoppingCart className="h-4 w-4" />
            {cart.length} আইটেম
          </div>
        </div>
      </header>

      <section className="mx-auto grid max-w-7xl gap-6 px-4 py-8 lg:grid-cols-[1.3fr_0.7fr]">
        <motion.div
          initial={{ opacity: 0, y: 16 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.5 }}
          className="overflow-hidden rounded-3xl bg-emerald-600 p-8 text-white shadow-lg"
        >
          <div className="flex flex-wrap items-center gap-2 text-sm">
            <Badge className="bg-white/15 text-white hover:bg-white/20">
              <Sparkles className="mr-1 h-3.5 w-3.5" /> আজকের অফার
            </Badge>
            <Badge className="bg-white/15 text-white hover:bg-white/20">
              <BadgePercent className="mr-1 h-3.5 w-3.5" /> ₹499-এর বেশি হলে ফ্রি ডেলিভারি
            </Badge>
          </div>
          <h2 className="mt-4 text-3xl font-extrabold leading-tight md:text-5xl">
            ঘরে বসেই কিনুন একদম তাজা সবজি
          </h2>
          <p className="mt-4 max-w-2xl text-base text-emerald-50 md:text-lg">
            প্রতিদিনের প্রয়োজনীয় সবজি, খুচরা ও পাইকারি দামে। অর্ডার করুন, আর দ্রুত ডেলিভারি নিন আপনার দরজায়।
          </p>
          <div className="mt-6 flex flex-wrap gap-3">
            <Button className="rounded-2xl bg-white text-emerald-700 hover:bg-emerald-50">
              এখনই কেনাকাটা করুন
              <ChevronRight className="ml-1 h-4 w-4" />
            </Button>
            <Button variant="outline" className="rounded-2xl border-white/40 bg-transparent text-white hover:bg-white/10">
              আজকের ডিল দেখুন
            </Button>
          </div>
          <div className="mt-8 grid gap-3 sm:grid-cols-3">
            {[
              ["১০ মিনিটে প্যাকিং", Truck],
              ["১০০% ফ্রেশ", ShieldCheck],
              ["সেরা দামে", Star],
            ].map(([label, Icon]) => (
              <div key={label} className="rounded-2xl bg-white/10 p-4 backdrop-blur">
                <Icon className="h-5 w-5" />
                <p className="mt-2 text-sm font-semibold">{label}</p>
              </div>
            ))}
          </div>
        </motion.div>

        <Card className="rounded-3xl border-0 shadow-lg">
          <CardHeader>
            <CardTitle className="text-xl">আজকের সারাংশ</CardTitle>
          </CardHeader>
          <CardContent className="space-y-4">
            <div className="rounded-2xl bg-slate-50 p-4">
              <div className="text-sm text-slate-500">মোট পণ্য</div>
              <div className="text-2xl font-bold">{products.length}</div>
            </div>
            <div className="rounded-2xl bg-slate-50 p-4">
              <div className="text-sm text-slate-500">কার্ট</div>
              <div className="text-2xl font-bold">{cart.length} আইটেম</div>
            </div>
            <div className="rounded-2xl bg-slate-50 p-4">
              <div className="text-sm text-slate-500">ডেলিভারি</div>
              <div className="text-2xl font-bold">{deliveryFee === 0 ? "ফ্রি" : `₹${deliveryFee}`}</div>
            </div>
          </CardContent>
        </Card>
      </section>

      <main className="mx-auto grid max-w-7xl gap-6 px-4 pb-10 lg:grid-cols-[1fr_360px]">
        <section className="space-y-6">
          <Card className="rounded-3xl border-0 shadow-md">
            <CardContent className="p-4 md:p-6">
              <div className="grid gap-3 md:grid-cols-[1fr_auto] md:items-center">
                <div className="relative">
                  <Search className="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-slate-400" />
                  <Input
                    value={query}
                    onChange={(e) => setQuery(e.target.value)}
                    placeholder="সবজি খুঁজুন..."
                    className="h-12 rounded-2xl pl-10"
                  />
                </div>
                <div className="flex flex-wrap gap-2">
                  {categories.map((cat) => (
                    <Button
                      key={cat}
                      onClick={() => setCategory(cat)}
                      variant={category === cat ? "default" : "outline"}
                      className={`rounded-2xl ${category === cat ? "bg-emerald-600 hover:bg-emerald-700" : ""}`}
                    >
                      {cat}
                    </Button>
                  ))}
                </div>
              </div>
            </CardContent>
          </Card>

          <div className="grid gap-4 sm:grid-cols-2 xl:grid-cols-3">
            {filteredProducts.map((product) => (
              <motion.div
                key={product.id}
                initial={{ opacity: 0, scale: 0.96 }}
                animate={{ opacity: 1, scale: 1 }}
                transition={{ duration: 0.25 }}
              >
                <Card className="h-full rounded-3xl border-0 shadow-md transition hover:-translate-y-1 hover:shadow-xl">
                  <CardContent className="p-5">
                    <div className="flex items-start justify-between gap-3">
                      <img src={product.image} className="h-32 w-full object-cover rounded-2xl" />
                      <Badge variant="secondary" className="rounded-full">
                        {product.fresh}
                      </Badge>
                    </div>
                    <h3 className="mt-4 text-xl font-bold">{product.name}</h3>
                    <p className="mt-1 text-sm text-slate-500">{product.category}</p>
                    <div className="mt-4 flex items-center justify-between">
                      <div>
                        <div className="text-2xl font-extrabold text-emerald-700">₹{product.price}</div>
                        <div className="text-sm text-slate-500">প্রতি {product.unit}</div>
                      </div>
                      <div className="flex items-center gap-1 rounded-full bg-amber-50 px-3 py-1 text-sm font-semibold text-amber-700">
                        <Star className="h-4 w-4 fill-current" /> {product.rating}
                      </div>
                    </div>
                    <Button
                      onClick={() => addToCart(product)}
                      className="mt-5 h-11 w-full rounded-2xl bg-emerald-600 hover:bg-emerald-700"
                    >
                      কার্টে যোগ করুন
                    </Button>
                  </CardContent>
                </Card>
              </motion.div>
            ))}
          </div>

          {filteredProducts.length === 0 && (
            <Card className="rounded-3xl border-0 shadow-md">
              <CardContent className="p-10 text-center text-slate-500">
                কোনো সবজি পাওয়া যায়নি। অন্য নাম বা ক্যাটেগরি চেষ্টা করুন।
              </CardContent>
            </Card>
          )}
        </section>

        <aside className="space-y-6">
          <Card className="rounded-3xl border-0 shadow-md">
            <CardHeader>
              <CardTitle className="flex items-center gap-2 text-xl">
                <ShoppingCart className="h-5 w-5 text-emerald-600" /> কার্ট
              </CardTitle>
            </CardHeader>
            <CardContent className="space-y-4">
              {cart.length === 0 ? (
                <div className="rounded-2xl border border-dashed p-6 text-center text-sm text-slate-500">
                  কার্ট এখন খালি। কিছু তাজা সবজি যোগ করুন।
                </div>
              ) : (
                <div className="space-y-3">
                  {cart.map((item) => (
                    <div key={item.id} className="rounded-2xl border p-3">
                      <div className="flex items-start justify-between gap-2">
                        <div>
                          <div className="font-semibold">{item.name}</div>
                          <div className="text-sm text-slate-500">₹{item.price} × {item.qty}</div>
                        </div>
                        <button onClick={() => removeItem(item.id)} className="text-slate-400 hover:text-red-500">
                          <Trash2 className="h-4 w-4" />
                        </button>
                      </div>
                      <div className="mt-3 flex items-center justify-between">
                        <div className="flex items-center gap-2">
                          <Button variant="outline" size="icon" className="h-8 w-8 rounded-full" onClick={() => updateQty(item.id, -1)}>
                            <Minus className="h-4 w-4" />
                          </Button>
                          <span className="min-w-6 text-center text-sm font-semibold">{item.qty}</span>
                          <Button variant="outline" size="icon" className="h-8 w-8 rounded-full" onClick={() => updateQty(item.id, 1)}>
                            <Plus className="h-4 w-4" />
                          </Button>
                        </div>
                        <div className="font-bold text-emerald-700">₹{item.price * item.qty}</div>
                      </div>
                    </div>
                  ))}
                </div>
              )}

              <Separator />

              <div className="space-y-2 text-sm">
                <div className="flex items-center justify-between">
                  <span>সাবটোটাল</span>
                  <span>₹{total}</span>
                </div>
                <div className="flex items-center justify-between">
                  <span>ডেলিভারি চার্জ</span>
                  <span>{deliveryFee === 0 ? "ফ্রি" : `₹${deliveryFee}`}</span>
                </div>
                <div className="flex items-center justify-between text-base font-bold">
                  <span>মোট</span>
                  <span>₹{grandTotal}</span>
                </div>
              </div>

              <a
                href={`https://wa.me/918101925969?text=${encodeURIComponent(`🛒 Sankar Vegetable Store

📦 Order:
${cart.map(i=>`${i.name} - ${i.qty}`).join("
")}

💰 Total: ₹${grandTotal}
📍 Location: Ambari
🚚 Delivery: ₹50

🙏 Please confirm order`)}`}
                target="_blank"
                rel="noreferrer"
              >
                <Button className="h-12 w-full rounded-2xl bg-emerald-600 hover:bg-emerald-700" disabled={cart.length === 0}>
                  WhatsApp-এ অর্ডার করুন
                </Button>
              </a>
            </CardContent>
          </Card>

          <Card className="rounded-3xl border-0 shadow-md">
            <CardHeader>
              <CardTitle className="text-lg">আমাদের সুবিধা</CardTitle>
            </CardHeader>
            <CardContent className="space-y-3 text-sm text-slate-600">
              <div className="flex items-center gap-3 rounded-2xl bg-slate-50 p-3">
                <Truck className="h-5 w-5 text-emerald-600" />
                দ্রুত হোম ডেলিভারি
              </div>
              <div className="flex items-center gap-3 rounded-2xl bg-slate-50 p-3">
                <ShieldCheck className="h-5 w-5 text-emerald-600" />
                মানসম্মত ও নিরাপদ পণ্য
              </div>
              <div className="flex items-center gap-3 rounded-2xl bg-slate-50 p-3">
                <Leaf className="h-5 w-5 text-emerald-600" />
                ফার্ম-ফ্রেশ সংগ্রহ
              </div>
            </CardContent>
          </Card>
        </aside>
      </main>
    </div>
  );
}
