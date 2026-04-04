import { useState, useEffect, useRef } from "react";

const COLORS = {
  navyDeep: "#0D2B5E",
  navyMid: "#1A3A7A",
  navyLight: "#2952A3",
  blue: "#1565C0",
  blueLight: "#1976D2",
  accent: "#0288D1",
  green: "#2E7D32",
  greenLight: "#43A047",
  greenBright: "#00C853",
  greenBg: "#E8F5E9",
  red: "#C62828",
  redBg: "#FFEBEE",
  amber: "#F57F17",
  amberBg: "#FFF8E1",
  white: "#FFFFFF",
  bg: "#F0F4FA",
  card: "#FFFFFF",
  border: "#CFD8DC",
  textPrimary: "#0D1B2E",
  textSecondary: "#546E7A",
  textMuted: "#90A4AE",
};

const CITY_DATA = [
  { city: "Delhi", aqi: 387, rain: "Clear", temp: 38, trigger: true },
  { city: "Mumbai", aqi: 142, rain: "Heavy", temp: 29, trigger: false },
  { city: "Bangalore", aqi: 95, rain: "Light", temp: 24, trigger: false },
  { city: "Chennai", aqi: 178, rain: "Moderate", temp: 33, trigger: false },
  { city: "Kolkata", aqi: 312, rain: "Clear", temp: 36, trigger: true },
  { city: "Hyderabad", aqi: 201, rain: "Clear", temp: 35, trigger: false },
];

const PLATFORMS = ["Zomato", "Swiggy", "Zepto", "Blinkit", "Dunzo"];

const PAYOUTS = [
  { date: "28 Jun", reason: "Delhi AQI: 387", amount: 500, status: "Paid" },
  { date: "21 Jun", reason: "Delhi AQI: 341", amount: 500, status: "Paid" },
  { date: "14 Jun", reason: "Kolkata AQI: 318", amount: 1000, status: "Paid" },
  { date: "07 Jun", reason: "Delhi AQI: 356", amount: 500, status: "Paid" },
];

const POOL_STATS = {
  totalPremiums: 284500,
  totalPayouts: 241825,
  lossRatio: 85.0,
  enrollments: 5690,
  suspended: false,
};

function StatusDot({ active }) {
  return (
    <span style={{
      display: "inline-block",
      width: 10, height: 10,
      borderRadius: "50%",
      background: active ? COLORS.greenBright : COLORS.red,
      boxShadow: active ? `0 0 6px ${COLORS.greenBright}` : "none",
      marginRight: 6,
    }} />
  );
}

function Badge({ label, color = "green" }) {
  const palettes = {
    green: { bg: COLORS.greenBg, text: COLORS.green },
    red: { bg: COLORS.redBg, text: COLORS.red },
    amber: { bg: COLORS.amberBg, text: COLORS.amber },
    blue: { bg: "#E3F2FD", text: COLORS.blue },
  };
  const p = palettes[color];
  return (
    <span style={{
      background: p.bg, color: p.text,
      fontSize: 11, fontWeight: 600, letterSpacing: "0.04em",
      padding: "3px 9px", borderRadius: 20, display: "inline-flex", alignItems: "center",
    }}>{label}</span>
  );
}

function Step({ num, label, active, done }) {
  return (
    <div style={{ display: "flex", flexDirection: "column", alignItems: "center", gap: 4 }}>
      <div style={{
        width: 36, height: 36, borderRadius: "50%",
        background: done ? COLORS.greenLight : active ? COLORS.navyMid : COLORS.border,
        color: done || active ? "#fff" : COLORS.textMuted,
        display: "flex", alignItems: "center", justifyContent: "center",
        fontSize: done ? 16 : 14, fontWeight: 600, transition: "all 0.3s",
      }}>
        {done ? "✓" : num}
      </div>
      <span style={{ fontSize: 11, color: active ? COLORS.navyMid : COLORS.textMuted, fontWeight: active ? 600 : 400, textAlign: "center" }}>{label}</span>
    </div>
  );
}

function ProgressBar({ value, max, color }) {
  const pct = Math.min(100, (value / max) * 100);
  const barColor = pct >= 85 ? COLORS.red : pct >= 70 ? COLORS.amber : color || COLORS.greenLight;
  return (
    <div style={{ background: COLORS.border, borderRadius: 8, height: 8, overflow: "hidden" }}>
      <div style={{ width: `${pct}%`, height: "100%", background: barColor, borderRadius: 8, transition: "width 0.6s ease" }} />
    </div>
  );
}

function AQIGauge({ value }) {
  const color = value > 300 ? COLORS.red : value > 200 ? COLORS.amber : COLORS.green;
  const label = value > 300 ? "Hazardous" : value > 200 ? "Very Poor" : value > 100 ? "Poor" : "Moderate";
  return (
    <div style={{ textAlign: "center" }}>
      <div style={{
        fontSize: 42, fontWeight: 700, color, lineHeight: 1,
        fontFamily: "'Courier New', monospace"
      }}>{value}</div>
      <div style={{ fontSize: 11, color, fontWeight: 600, marginTop: 2 }}>{label}</div>
    </div>
  );
}

function Card({ children, style = {} }) {
  return (
    <div style={{
      background: COLORS.card, borderRadius: 14,
      border: `1.5px solid ${COLORS.border}`,
      padding: "20px 22px",
      ...style,
    }}>
      {children}
    </div>
  );
}

function SectionTitle({ children, sub }) {
  return (
    <div style={{ marginBottom: 6 }}>
      <div style={{ fontSize: 13, fontWeight: 700, color: COLORS.textSecondary, letterSpacing: "0.08em", textTransform: "uppercase" }}>{children}</div>
      {sub && <div style={{ fontSize: 12, color: COLORS.textMuted, marginTop: 2 }}>{sub}</div>}
    </div>
  );
}

// ─── Onboarding ───────────────────────────────────────
function Onboarding({ onComplete }) {
  const [step, setStep] = useState(1);
  const [form, setForm] = useState({ name: "", phone: "", platform: "", city: "", aadhaar: "", plan: "" });
  const [loading, setLoading] = useState(false);

  const steps = ["Identity", "Platform", "Plan", "Verify"];
  const planOptions = [
    { id: "basic", label: "Basic Shield", premium: 20, cover: "AQI Trigger", color: COLORS.accent },
    { id: "plus", label: "Plus Guard", premium: 35, cover: "AQI + Rain", color: COLORS.navyMid },
    { id: "pro", label: "Pro Armor", premium: 50, cover: "AQI + Rain + Heat", color: COLORS.green, popular: true },
  ];

  const next = () => {
    if (step === 4) {
      setLoading(true);
      setTimeout(() => { setLoading(false); onComplete(form); }, 1800);
    } else setStep(s => s + 1);
  };

  const inp = (field) => ({
    value: form[field],
    onChange: e => setForm(f => ({ ...f, [field]: e.target.value })),
    style: {
      width: "100%", padding: "11px 14px", border: `1.5px solid ${COLORS.border}`,
      borderRadius: 9, fontSize: 14, outline: "none", boxSizing: "border-box",
      background: "#F8FBFF", color: COLORS.textPrimary,
    }
  });

  return (
    <div style={{ minHeight: "100vh", background: COLORS.bg, display: "flex", alignItems: "center", justifyContent: "center", padding: 16 }}>
      <div style={{ width: "100%", maxWidth: 480 }}>
        {/* Header */}
        <div style={{ textAlign: "center", marginBottom: 28 }}>
          <div style={{ display: "inline-flex", alignItems: "center", gap: 10, background: COLORS.navyDeep, color: "#fff", padding: "10px 20px", borderRadius: 40, marginBottom: 14 }}>
            <span style={{ fontSize: 20 }}>🛡️</span>
            <span style={{ fontSize: 16, fontWeight: 700, letterSpacing: "0.02em" }}>GigShield</span>
          </div>
          <div style={{ fontSize: 22, fontWeight: 700, color: COLORS.navyDeep, lineHeight: 1.3 }}>Parametric Protection for Gig Workers</div>
          <div style={{ fontSize: 14, color: COLORS.textSecondary, marginTop: 6 }}>Auto-payouts when AQI exceeds 300. Zero paperwork.</div>
        </div>

        {/* Stepper */}
        <div style={{ display: "flex", alignItems: "center", justifyContent: "center", gap: 0, marginBottom: 24 }}>
          {steps.map((s, i) => (
            <div key={s} style={{ display: "flex", alignItems: "center" }}>
              <Step num={i + 1} label={s} active={step === i + 1} done={step > i + 1} />
              {i < steps.length - 1 && (
                <div style={{ width: 40, height: 2, background: step > i + 1 ? COLORS.greenLight : COLORS.border, margin: "0 6px", marginTop: -18 }} />
              )}
            </div>
          ))}
        </div>

        <Card>
          {step === 1 && (
            <div style={{ display: "flex", flexDirection: "column", gap: 14 }}>
              <SectionTitle>Personal Details</SectionTitle>
              <div>
                <label style={{ fontSize: 12, color: COLORS.textSecondary, marginBottom: 5, display: "block" }}>Full Name</label>
                <input placeholder="Ravi Kumar" {...inp("name")} />
              </div>
              <div>
                <label style={{ fontSize: 12, color: COLORS.textSecondary, marginBottom: 5, display: "block" }}>Mobile Number</label>
                <input placeholder="+91 98765 43210" {...inp("phone")} />
              </div>
              <div>
                <label style={{ fontSize: 12, color: COLORS.textSecondary, marginBottom: 5, display: "block" }}>City</label>
                <select {...inp("city")} style={{ ...inp("city").style }}>
                  <option value="">Select city</option>
                  {CITY_DATA.map(c => <option key={c.city}>{c.city}</option>)}
                </select>
              </div>
            </div>
          )}

          {step === 2 && (
            <div style={{ display: "flex", flexDirection: "column", gap: 14 }}>
              <SectionTitle>Platform Connection</SectionTitle>
              <div>
                <label style={{ fontSize: 12, color: COLORS.textSecondary, marginBottom: 8, display: "block" }}>Select your gig platform</label>
                <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10 }}>
                  {PLATFORMS.map(p => (
                    <div key={p} onClick={() => setForm(f => ({ ...f, platform: p }))}
                      style={{
                        padding: "12px 14px", borderRadius: 10, cursor: "pointer",
                        border: `2px solid ${form.platform === p ? COLORS.navyMid : COLORS.border}`,
                        background: form.platform === p ? "#EEF2FF" : "#fff",
                        textAlign: "center", fontSize: 14, fontWeight: 600, color: form.platform === p ? COLORS.navyMid : COLORS.textSecondary,
                        transition: "all 0.2s",
                      }}>
                      {p}
                    </div>
                  ))}
                </div>
              </div>
              <div>
                <label style={{ fontSize: 12, color: COLORS.textSecondary, marginBottom: 5, display: "block" }}>Aadhaar (last 4 digits)</label>
                <input placeholder="XXXX" maxLength={4} {...inp("aadhaar")} />
              </div>
            </div>
          )}

          {step === 3 && (
            <div>
              <SectionTitle sub="Select weekly premium plan">Choose Your Plan</SectionTitle>
              <div style={{ display: "flex", flexDirection: "column", gap: 12, marginTop: 14 }}>
                {planOptions.map(plan => (
                  <div key={plan.id} onClick={() => setForm(f => ({ ...f, plan: plan.id }))}
                    style={{
                      padding: "16px 18px", borderRadius: 12, cursor: "pointer",
                      border: `2px solid ${form.plan === plan.id ? plan.color : COLORS.border}`,
                      background: form.plan === plan.id ? `${plan.color}11` : "#fff",
                      display: "flex", alignItems: "center", justifyContent: "space-between",
                      transition: "all 0.2s", position: "relative",
                    }}>
                    {plan.popular && (
                      <div style={{ position: "absolute", top: -10, right: 14, background: COLORS.greenLight, color: "#fff", fontSize: 10, fontWeight: 700, padding: "3px 10px", borderRadius: 20 }}>POPULAR</div>
                    )}
                    <div>
                      <div style={{ fontWeight: 700, color: COLORS.textPrimary, fontSize: 15 }}>{plan.label}</div>
                      <div style={{ fontSize: 12, color: COLORS.textSecondary, marginTop: 3 }}>Covers: {plan.cover}</div>
                      <div style={{ fontSize: 12, color: COLORS.textMuted, marginTop: 2 }}>Payout: ₹500/trigger day</div>
                    </div>
                    <div style={{ textAlign: "right" }}>
                      <div style={{ fontSize: 22, fontWeight: 700, color: plan.color }}>₹{plan.premium}</div>
                      <div style={{ fontSize: 11, color: COLORS.textMuted }}>/week</div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {step === 4 && (
            <div>
              <SectionTitle sub="We're verifying your details">Identity Verification</SectionTitle>
              <div style={{ display: "flex", flexDirection: "column", gap: 14, marginTop: 16 }}>
                {[
                  { label: "Aadhaar Verification", desc: "Cross-checking with UIDAI", status: "Verified" },
                  { label: "Platform Login Data", desc: `${form.platform || "Platform"} account linked`, status: "Active" },
                  { label: "GPS Location Check", desc: `City: ${form.city || "Unknown"}`, status: "In Zone" },
                  { label: "Risk Pool Eligibility", desc: "Pool capacity available", status: "Eligible" },
                ].map((item, i) => (
                  <div key={i} style={{ display: "flex", alignItems: "center", justifyContent: "space-between", padding: "12px 14px", background: COLORS.greenBg, borderRadius: 10 }}>
                    <div>
                      <div style={{ fontWeight: 600, fontSize: 14, color: COLORS.textPrimary }}>{item.label}</div>
                      <div style={{ fontSize: 12, color: COLORS.textSecondary }}>{item.desc}</div>
                    </div>
                    <div style={{ display: "flex", alignItems: "center", gap: 6, color: COLORS.green, fontWeight: 700, fontSize: 13 }}>
                      <span style={{ fontSize: 16 }}>✓</span> {item.status}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          <button
            onClick={next}
            disabled={loading}
            style={{
              width: "100%", marginTop: 22, padding: "13px",
              background: loading ? COLORS.border : `linear-gradient(135deg, ${COLORS.navyMid}, ${COLORS.navyLight})`,
              color: "#fff", border: "none", borderRadius: 10,
              fontSize: 15, fontWeight: 700, cursor: loading ? "not-allowed" : "pointer",
              letterSpacing: "0.03em", transition: "all 0.2s",
            }}>
            {loading ? "Activating Protection..." : step === 4 ? "Activate Protection 🛡️" : "Continue →"}
          </button>
        </Card>

        <div style={{ textAlign: "center", marginTop: 14, fontSize: 12, color: COLORS.textMuted }}>
          256-bit encrypted · IRDAI compliant · Instant payouts via UPI
        </div>
      </div>
    </div>
  );
}

// ─── Dashboard ────────────────────────────────────────
function Dashboard({ worker }) {
  const [liveTime, setLiveTime] = useState(new Date());
  const [selectedCity, setSelectedCity] = useState(worker.city || "Delhi");
  const [counter, setCounter] = useState(0);
  const cityData = CITY_DATA.find(c => c.city === selectedCity) || CITY_DATA[0];
  const totalEarned = PAYOUTS.reduce((a, p) => a + p.amount, 0);

  useEffect(() => {
    const t = setInterval(() => setLiveTime(new Date()), 1000);
    return () => clearInterval(t);
  }, []);

  useEffect(() => {
    let start = 0;
    const inc = setInterval(() => {
      start += 87;
      if (start >= totalEarned) { setCounter(totalEarned); clearInterval(inc); }
      else setCounter(start);
    }, 20);
    return () => clearInterval(inc);
  }, []);

  const planLabel = worker.plan === "pro" ? "Pro Armor" : worker.plan === "plus" ? "Plus Guard" : "Basic Shield";
  const premium = worker.plan === "pro" ? 50 : worker.plan === "plus" ? 35 : 20;

  return (
    <div style={{ minHeight: "100vh", background: COLORS.bg }}>
      {/* Top Nav */}
      <div style={{ background: COLORS.navyDeep, padding: "0 20px" }}>
        <div style={{ maxWidth: 900, margin: "0 auto", display: "flex", alignItems: "center", justifyContent: "space-between", height: 58 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
            <span style={{ fontSize: 20 }}>🛡️</span>
            <span style={{ color: "#fff", fontWeight: 700, fontSize: 17, letterSpacing: "0.02em" }}>GigShield</span>
          </div>
          <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
            <StatusDot active />
            <span style={{ color: "#A5D6A7", fontSize: 12, fontWeight: 600 }}>LIVE</span>
            <span style={{ color: "#78909C", fontSize: 12, marginLeft: 8 }}>{liveTime.toLocaleTimeString("en-IN")}</span>
          </div>
          <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
            <div style={{ width: 34, height: 34, borderRadius: "50%", background: COLORS.navyLight, display: "flex", alignItems: "center", justifyContent: "center", color: "#fff", fontWeight: 700, fontSize: 14 }}>
              {(worker.name || "R")[0].toUpperCase()}
            </div>
            <div>
              <div style={{ color: "#fff", fontSize: 13, fontWeight: 600 }}>{worker.name || "Ravi Kumar"}</div>
              <div style={{ color: "#78909C", fontSize: 11 }}>{worker.platform || "Zomato"} · {selectedCity}</div>
            </div>
          </div>
        </div>
      </div>

      {/* Main */}
      <div style={{ maxWidth: 900, margin: "0 auto", padding: "20px 16px" }}>

        {/* Pool Suspension Banner */}
        {POOL_STATS.suspended && (
          <div style={{ background: COLORS.redBg, border: `1.5px solid ${COLORS.red}`, borderRadius: 10, padding: "14px 18px", marginBottom: 18, display: "flex", alignItems: "center", gap: 12 }}>
            <span style={{ fontSize: 22 }}>⚠️</span>
            <div>
              <div style={{ fontWeight: 700, color: COLORS.red, fontSize: 15 }}>Pool Suspended — New Enrollments Disabled</div>
              <div style={{ color: COLORS.red, fontSize: 13 }}>Loss ratio has exceeded 85%. Existing policyholders remain covered.</div>
            </div>
          </div>
        )}

        {/* Row 1: Active Protection + Live Risk */}
        <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 16, marginBottom: 16 }}>

          {/* Active Protection */}
          <Card style={{ borderTop: `4px solid ${COLORS.greenLight}` }}>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 14 }}>
              <SectionTitle>Active Protection</SectionTitle>
              <Badge label="● ACTIVE" color="green" />
            </div>
            <div style={{ fontSize: 22, fontWeight: 800, color: COLORS.navyDeep, marginBottom: 2 }}>{planLabel}</div>
            <div style={{ fontSize: 13, color: COLORS.textSecondary, marginBottom: 16 }}>AQI trigger cover · ₹500/trigger day</div>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10 }}>
              {[
                { label: "Weekly Premium", value: `₹${premium}` },
                { label: "Policy ID", value: "GS-" + Math.abs(worker.phone || "9876").toString().slice(-5) },
                { label: "Platform", value: worker.platform || "Zomato" },
                { label: "Valid Till", value: "05 Jul 2025" },
              ].map(({ label, value }) => (
                <div key={label} style={{ background: COLORS.bg, borderRadius: 9, padding: "10px 12px" }}>
                  <div style={{ fontSize: 11, color: COLORS.textMuted }}>{label}</div>
                  <div style={{ fontSize: 14, fontWeight: 700, color: COLORS.textPrimary, marginTop: 2 }}>{value}</div>
                </div>
              ))}
            </div>
          </Card>

          {/* Live Risk Monitor */}
          <Card style={{ borderTop: `4px solid ${cityData.trigger ? COLORS.red : COLORS.accent}` }}>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 10 }}>
              <SectionTitle>Live Risk Monitor</SectionTitle>
              <select value={selectedCity} onChange={e => setSelectedCity(e.target.value)}
                style={{ fontSize: 12, border: `1px solid ${COLORS.border}`, borderRadius: 6, padding: "4px 8px", background: "#fff", color: COLORS.textPrimary }}>
                {CITY_DATA.map(c => <option key={c.city}>{c.city}</option>)}
              </select>
            </div>

            <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 14 }}>
              <AQIGauge value={cityData.aqi} />
              <div style={{ textAlign: "right" }}>
                <div style={{ fontSize: 12, color: COLORS.textMuted, marginBottom: 6 }}>🌧 Rain: <strong>{cityData.rain}</strong></div>
                <div style={{ fontSize: 12, color: COLORS.textMuted, marginBottom: 6 }}>🌡 Temp: <strong>{cityData.temp}°C</strong></div>
                <div style={{ fontSize: 12, color: COLORS.textMuted }}>🕐 Updated: Just now</div>
              </div>
            </div>

            <div style={{ background: cityData.trigger ? COLORS.redBg : COLORS.greenBg, borderRadius: 10, padding: "10px 14px", display: "flex", alignItems: "center", gap: 10 }}>
              <span style={{ fontSize: 18 }}>{cityData.trigger ? "🚨" : "✅"}</span>
              <div>
                <div style={{ fontWeight: 700, fontSize: 13, color: cityData.trigger ? COLORS.red : COLORS.green }}>
                  {cityData.trigger ? "TRIGGER ACTIVE — Payout Processing" : "No Trigger — Normal Conditions"}
                </div>
                <div style={{ fontSize: 11, color: COLORS.textSecondary }}>
                  {cityData.trigger ? `AQI ${cityData.aqi} > threshold 300` : `AQI ${cityData.aqi} below threshold 300`}
                </div>
              </div>
            </div>
          </Card>
        </div>

        {/* Row 2: Earnings Counter + Fraud Verification */}
        <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 16, marginBottom: 16 }}>

          {/* Earnings Protected */}
          <Card>
            <SectionTitle sub="Auto-triggered, zero-claim payouts">Earnings Protected</SectionTitle>
            <div style={{ textAlign: "center", padding: "18px 0" }}>
              <div style={{ fontSize: 46, fontWeight: 800, color: COLORS.green, letterSpacing: "-1px", fontFamily: "'Courier New', monospace" }}>
                ₹{counter.toLocaleString("en-IN")}
              </div>
              <div style={{ fontSize: 13, color: COLORS.textMuted, marginTop: 4 }}>Total received this year</div>
            </div>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr 1fr", gap: 8 }}>
              {[
                { label: "Payouts", value: "4" },
                { label: "Trigger Days", value: "5" },
                { label: "Avg Days", value: "1.25" },
              ].map(({ label, value }) => (
                <div key={label} style={{ background: COLORS.bg, borderRadius: 8, padding: "10px 10px", textAlign: "center" }}>
                  <div style={{ fontSize: 18, fontWeight: 700, color: COLORS.navyDeep }}>{value}</div>
                  <div style={{ fontSize: 11, color: COLORS.textMuted }}>{label}</div>
                </div>
              ))}
            </div>
          </Card>

          {/* Fraud & Verification */}
          <Card>
            <SectionTitle sub="Real-time cross-check status">Fraud & Verification</SectionTitle>
            <div style={{ display: "flex", flexDirection: "column", gap: 10, marginTop: 10 }}>
              {[
                { label: "GPS Cross-Check", detail: `User in ${selectedCity} trigger zone`, ok: true },
                { label: "Platform Login Data", detail: `${worker.platform || "Zomato"}: User Active`, ok: true },
                { label: "Aadhaar Link", detail: "UIDAI: Verified ✓", ok: true },
                { label: "Device Fingerprint", detail: "Single device confirmed", ok: true },
                { label: "Duplicate Policy Check", detail: "No duplicates found", ok: true },
              ].map(({ label, detail, ok }) => (
                <div key={label} style={{ display: "flex", alignItems: "center", justifyContent: "space-between", padding: "10px 12px", background: COLORS.bg, borderRadius: 9 }}>
                  <div>
                    <div style={{ fontSize: 13, fontWeight: 600, color: COLORS.textPrimary }}>{label}</div>
                    <div style={{ fontSize: 11, color: COLORS.textSecondary }}>{detail}</div>
                  </div>
                  <div style={{ display: "flex", alignItems: "center", gap: 4, color: ok ? COLORS.green : COLORS.red, fontWeight: 700, fontSize: 13 }}>
                    <span style={{ fontSize: 16 }}>{ok ? "✓" : "✗"}</span>
                    <span style={{ fontSize: 11 }}>{ok ? "OK" : "FAIL"}</span>
                  </div>
                </div>
              ))}
            </div>
          </Card>
        </div>

        {/* Row 3: Recent Payouts + Pool Health */}
        <div style={{ display: "grid", gridTemplateColumns: "1.4fr 1fr", gap: 16, marginBottom: 16 }}>

          {/* Recent Payouts */}
          <Card>
            <SectionTitle sub="Auto-processed via UPI">Recent Payouts</SectionTitle>
            <div style={{ marginTop: 14 }}>
              {PAYOUTS.map((p, i) => (
                <div key={i} style={{
                  display: "flex", alignItems: "center", justifyContent: "space-between",
                  padding: "12px 14px", background: i % 2 === 0 ? COLORS.bg : "#fff",
                  borderRadius: 9, marginBottom: 6,
                }}>
                  <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                    <div style={{ width: 38, height: 38, borderRadius: "50%", background: COLORS.greenBg, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 16 }}>💸</div>
                    <div>
                      <div style={{ fontWeight: 600, fontSize: 13, color: COLORS.textPrimary }}>{p.reason}</div>
                      <div style={{ fontSize: 11, color: COLORS.textMuted }}>{p.date} · Auto-trigger payout</div>
                    </div>
                  </div>
                  <div style={{ textAlign: "right" }}>
                    <div style={{ fontWeight: 700, fontSize: 15, color: COLORS.green }}>+₹{p.amount}</div>
                    <Badge label={p.status} color="green" />
                  </div>
                </div>
              ))}
            </div>
          </Card>

          {/* Pool Health */}
          <Card>
            <SectionTitle sub="Actuarial risk pool status">Pool Health</SectionTitle>
            <div style={{ display: "flex", flexDirection: "column", gap: 14, marginTop: 14 }}>
              <div>
                <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 6 }}>
                  <span style={{ fontSize: 13, color: COLORS.textSecondary }}>Loss Ratio</span>
                  <span style={{ fontSize: 13, fontWeight: 700, color: POOL_STATS.lossRatio >= 85 ? COLORS.red : COLORS.green }}>
                    {POOL_STATS.lossRatio}%
                  </span>
                </div>
                <ProgressBar value={POOL_STATS.lossRatio} max={100} />
                <div style={{ fontSize: 11, color: COLORS.textMuted, marginTop: 4 }}>Suspension threshold: 85%</div>
              </div>

              {[
                { label: "Total Premiums Collected", value: `₹${(POOL_STATS.totalPremiums / 1000).toFixed(1)}K` },
                { label: "Total Payouts Made", value: `₹${(POOL_STATS.totalPayouts / 1000).toFixed(1)}K` },
                { label: "Active Enrollments", value: POOL_STATS.enrollments.toLocaleString("en-IN") },
              ].map(({ label, value }) => (
                <div key={label} style={{ display: "flex", justifyContent: "space-between", padding: "10px 12px", background: COLORS.bg, borderRadius: 9 }}>
                  <span style={{ fontSize: 12, color: COLORS.textSecondary }}>{label}</span>
                  <span style={{ fontSize: 13, fontWeight: 700, color: COLORS.textPrimary }}>{value}</span>
                </div>
              ))}

              <div style={{ padding: "10px 12px", background: COLORS.amberBg, borderRadius: 9, borderLeft: `3px solid ${COLORS.amber}` }}>
                <div style={{ fontSize: 12, fontWeight: 600, color: COLORS.amber }}>⚠ Near Suspension Threshold</div>
                <div style={{ fontSize: 11, color: COLORS.textSecondary, marginTop: 3 }}>Loss ratio is critically high. New enrollments may be paused soon.</div>
              </div>
            </div>
          </Card>
        </div>

        {/* Footer */}
        <div style={{ textAlign: "center", padding: "16px 0", fontSize: 12, color: COLORS.textMuted }}>
          GigShield · IRDAI Reg. No. GS/2024/001 · Powered by parametric triggers via CPCB SAMEER API
        </div>
      </div>
    </div>
  );
}

// ─── App Root ─────────────────────────────────────────
export default function App() {
  const [worker, setWorker] = useState(null);

  if (!worker) return <Onboarding onComplete={setWorker} />;
  return <Dashboard worker={worker} />;
}