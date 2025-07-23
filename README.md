# soniqai.app
mport { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { motion } from "framer-motion";

export default function LandingPage() {
  const [email, setEmail] = useState("");
  const [industry, setIndustry] = useState("");
  const [script, setScript] = useState("");
  const [loading, setLoading] = useState(false);

  const handleSubmit = async () => {
    setLoading(true);
    const res = await fetch("/api/generate-script", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email, industry })
    });
    const data = await res.json();
    setScript(data.script);
    setLoading(false);
  };

  return (
    <div className="p-8 max-w-5xl mx-auto">
      <motion.h1 initial={{ opacity: 0, y: -30 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.6 }} className="text-4xl font-bold mb-4 text-center">
        Make the Most of Email and Time with <span className="text-blue-500">SoniqAI</span>
      </motion.h1>

      <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ delay: 0.2 }} className="text-center text-lg mb-8">
        Email that actually works. Get verified B2B leads and high-performing scripts tailored to your audience.
      </motion.p>

      <Card className="mb-10">
        <CardContent className="p-6 grid gap-4">
          <Input placeholder="Your email address" value={email} onChange={e => setEmail(e.target.value)} />
          <Input placeholder="Target industry or audience" value={industry} onChange={e => setIndustry(e.target.value)} />
          <Button onClick={handleSubmit} disabled={loading}>
            {loading ? "Generating..." : "Generate My Lead + Script"}
          </Button>
          {script && <Textarea value={script} readOnly className="mt-4" />}
        </CardContent>
      </Card>

      <div className="grid md:grid-cols-3 gap-6 mb-10">
        <Card><CardContent className="p-4">✅ Verified B2B leads by reply intent</CardContent></Card>
        <Card><CardContent className="p-4">✍️ AI-generated personalized email scripts</CardContent></Card>
        <Card><CardContent className="p-4">📈 Designed to boost your outreach ROI</CardContent></Card>
      </div>

      <Card className="mb-12">
        <CardContent className="p-6">
          <h2 className="text-2xl font-semibold mb-4">Pricing</h2>
          <ul className="space-y-2">
            <li><strong>Starter:</strong> $29/mo – 50 leads/month</li>
            <li><strong>Pro:</strong> $99/mo – 250 leads/month + premium targeting</li>
            <li><strong>Enterprise:</strong> Custom plan – dedicated support + integrations</li>
          </ul>
        </CardContent>
      </Card>

      <Card>
        <CardContent className="p-6">
          <h2 className="text-2xl font-semibold mb-4">Contact Us</h2>
          <form className="grid gap-4">
            <Input placeholder="Your Name" />
            <Input placeholder="Your Email" />
            <Textarea placeholder="Message" />
            <Button>Send Message</Button>
          </form>
        </CardContent>
      </Card>
    </div>
  );
}
